## Spring security request flow:

```
User types username=john & password=secret in the form
   ↓
   ↓
Browser reads the form fields (name=value pairs)
   ↓
   ↓
Browser encodes them into:
   username=john&password=secret
   ↓
   ↓
Browser wraps them into an HTTP Request:

   POST /auth/login HTTP/1.1                        → Request Line `("Hey server, I am sending data to /auth/login using HTTP version 1.1")`
   Host: localhost:8080                             → Headers `(metadata ABOUT the request)`
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 27
                                                    → Blank Line
   username=john&password=secret                    → Body `(actual data OF the request)`
   ↓
   ↓
HTTP  → travels as plain text  → anyone can intercept and read credentials (NOT SAFE)
HTTPS → travels as encrypted bytes → no one can read in middle (SAFE)
   ↓
   ↓
Reaches the Server (Tomcat)
   ↓
   ↓
Raw HTTP Request arrives on port 8080
   ↓
   ↓
Tomcat reads the raw bytes
   ↓
   ↓
Tomcat parses:
   ├── Request Line  → method=POST, url=/auth/login
   ├── Headers       → Content-Type, Host, Content-Length
   └── Body          → username=john&password=secret
   ↓
   ↓
Tomcat creates HttpServletRequest object:
   ├── getMethod()      → "POST"
   ├── getRequestURI()  → "/auth/login"
   ├── getHeader("Host") → "localhost:8080"
   └── getParameter("username") → "john"
       getParameter("password") → "secret"
   ↓
   ↓
Tomcat creates empty HttpServletResponse object:
   ├── status code  → (empty)
   ├── headers      → (empty)
   └── body         → (empty)
   ↓
   ↓
Both objects passed into Servlet Filter Chain
   ↓
   ↓
The first filter they hit in "Servlet Filter Chain" is DelegatingFilterProxy.
     It receives the request and says:
      - I don't handle security myself, 
      - I will delegate this to FilterChainProxy 
      - which lives in Spring's Application Context
   ↓
   ↓
FilterChainProxy (It receives the request and checks the URL /auth/login against all registered SecurityFilterChains.)
   ↓
   ↓
POST /auth/login → goes through filter chain:
   UsernamePasswordAuthenticationFilter handles it
      1. Checks if request is POST /auth/login
      2. Extracts username & password from request
      3. Creates UsernamePasswordAuthenticationToken
         - principal = username
         - credentials = password
         - authenticated = false
      4. Sends this token to AuthenticationManager for authentication
   ↓
   ↓
AuthenticationManager receives the token
      - It checks which provider can handle this token
      - It finds DaoAuthenticationProvider
      - Then it forwards the request to that provider
   ↓
   ↓
DaoAuthenticationProvider receives the token
   - It calls CustomUserDetailsService to load user from DB
   - It gets UserDetails (username, password, roles)
      Then compares:
      input password vs DB password

      If correct:
         create new authenticated token (authenticated = true)

      If wrong:
         throw exception
   ↓
   ↓
After successful authentication:
   1. UsernamePasswordAuthenticationFilter receives authenticated token
   2. It stores it in SecurityContextHolder
      → user is now logged in
   3. Request continues to AuthorizationFilter
      → checks roles
   4. If success → redirect to /blog/home
   5. SecurityContext is stored → user remains logged in for next requests
```

### Session:
```
After authentication:

1. User stored in SecurityContextHolder (temporary)

2. When request ends:
   SecurityContextPersistenceFilter stores it into HttpSession

Now user is saved permanently for next requests
```

```
POST /auth/logout

1. LogoutFilter handles request

2. It clears SecurityContextHolder
   → user removed

3. It invalidates HttpSession
   → session deleted

4. Removes session cookie

5. Redirect to /blog/home


Login → store user in session
Logout → remove user + delete session
```


<br>

## Basics:

### 1. What is a Filter Chain?
- `Multiple filters connected together. The request passes through them one by one in order:`
   ```
   HttpServletRequest + HttpServletResponse
      ↓
   Filter 1
      ↓
   Filter 2
      ↓
   Filter 3
      ↓
   Filter N
      ↓
   Your Application (DispatcherServlet)
   ```


### 2. DelegatingFilterProxy
* **Its only job is to delegate the request to FilterChainProxy which lives in Spring's Application Context.**
* It lives in the Servlet Filter Chain
* **Why is this needed?**
   - Because Tomcat and Spring are two different worlds:
      ```
      Tomcat manages Servlet filters
      Spring manages its own beans in Application Context

      DelegatingFilterProxy connects these two worlds:
      ```

* Spring Boot automatically registers DelegatingFilterProxy into the Servlet Filter Chain.
   ```
   How?
   When your Spring Boot application starts:

   - Spring Boot's auto configuration kicks in
   - It sees spring-security is on the classpath
   - It automatically creates DelegatingFilterProxy
   - It registers it into Tomcat's Servlet Filter Chain
   ```

### 3. FilterChainProxy
* `When a request comes in, it checks the request URL and pick's the matching Security Filter Chain.`


### 4. Example: What YOUR config triggers
1. **Because of .authorizeHttpRequests(...)**

   - `👉 Spring adds:`
   - `AuthorizationFilter`
      → checks roles like AUTHOR / ADMIN

2. **Because of .formLogin(...)**

   - `👉 Spring adds:`
   - `UsernamePasswordAuthenticationFilter` <br>
      → handles /auth/login POST <br>
      Login page handling filter

3. **Always added (core filters)**
   - `Even if you don’t configure:`
   - `SecurityContextHolderFilter` <br>
      → stores user authentication <br>
      ExceptionTranslationFilter <br>
      → handles 401 / 403
   * **Table:**
      | Your Config               | What Spring Adds                     |
      | ------------------------- | ------------------------------------ |
      | `authorizeHttpRequests()` | AuthorizationFilter                  |
      | `formLogin()`             | UsernamePasswordAuthenticationFilter |
      | `exceptionHandling()`     | ExceptionTranslationFilter           |
      | `csrf()`                  | CsrfFilter (removed in your case)    |
      | `logout()`                | LogoutFilter                         |
      | (always)                  | SecurityContextHolderFilter          |
