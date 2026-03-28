## 1. Spring Security Filter Chain Flow:
```
1. Browser
   ↓
2. HTTP request sent (TCP)
   ↓
3. Tomcat (Servlet Container)
   ↓
4. Creates:
   - HttpServletRequest
   - HttpServletResponse
   ↓
5. Servlet Filter Chain starts
   ↓
6. (If Spring Security present)
   DelegatingFilterProxy  ← (registered as servlet filter)
       ↓
       SecurityFilterChain (Spring bean)
           ↓
           Multiple filters execute in order:
           
           → UsernamePasswordAuthenticationFilter (login)
           → BasicAuthenticationFilter (basic auth)
           → Authorization filters
           
           → (If authentication needed)
               ↓
               AuthenticationManager (ProviderManager)
                   ↓
                   AuthenticationProvider (validation)
                   ↓
               Authenticated object returned
           
           → Stored in SecurityContextHolder
       
       ↓
   Back to Servlet Filter Chain
   ↓
7. DispatcherServlet
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