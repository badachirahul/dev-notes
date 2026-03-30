
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
