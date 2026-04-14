## 1. Spring Security Filter Chain Flow:

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
   username=john&password=secret                    → Body `(actual data Of the request)`
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
   
      If password is correct: 
         1. DaoAuthenticationProvider creates NEW "UsernamePasswordAuthenticationToken"

         2. In this:
            principal = UserDetails
            credentials = null
            authenticated = true
            authorities = roles

         3. This token is returned

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

### 🔥 Same in clean words (interview-ready)
1. Browser sends request
2. Tomcat receives and creates request/response objects
3. Request enters Servlet Filter Chain
4. If Spring Security is present:
    - DelegatingFilterProxy delegates to SecurityFilterChain
    - Security filters perform authentication and authorization
    - AuthenticationManager and AuthenticationProvider are involved
    - Result stored in SecurityContextHolder
5. Request returns to filter chain
6. Finally reaches DispatcherServlet


## DelegatingFilterProxy:
- `DelegatingFilterProxy is a servlet filter that delegates request from the 'Servlet Filter Chain' to the Spring-managed SecurityFilterChain bean, because servlet filters cannot directly access Spring beans.`
- **`Delegating-` means forwarding the request handling to another component instead of processing it itself.**

## SecurityContext: 
* `SecurityContext is an interface that holds the Authentication object, and at runtime it is implemented by SecurityContextImpl.`