# Spring MVC Request Flow (Example Explanation)

Example Controller:

``` java
@Controller
public class ContactController {

    @GetMapping("/contact")
    public String showPage() {
        return "contact";
    }
}
```

------------------------------------------------------------------------

## 1. Browser

User opens:

    http://localhost:8080/contact

The browser sends an **HTTP request**:

    GET /contact

------------------------------------------------------------------------

## 2. Tomcat (Servlet Container)

Tomcat receives the HTTP request.

Tomcat checks **which servlet should handle the request**.

In a Spring MVC application, all requests are handled by:

    DispatcherServlet

Flow:

    Browser
       ↓
    Tomcat
       ↓
    DispatcherServlet

------------------------------------------------------------------------

## 3. DispatcherServlet (Front Controller)
`DispatcherServlet` is the **central controller of Spring MVC**.
* `The servlet container forwards the request to DispatcherServlet, and DispatcherServlet dispatches the request to the appropriate controller method.`<br>

* Responsibilities:
    1.  Receive the HTTP request
    2.  Find the correct controller
    3.  Call the controller method
    4.  Return the response

It asks **HandlerMapping**:

    Which controller handles /contact ?

Spring scans controller annotations and finds:

    @GetMapping("/contact")

So it chooses:

    ContactController.showPage()

------------------------------------------------------------------------

## 4. Controller

Spring now calls the controller method:

``` java
public String showPage() {
    return "contact";
}
```

Controller responsibilities:

-   Handle request
-   Call service layer if needed
-   Return view name or data

Here:

    return "contact"

This means:

    View name = contact

------------------------------------------------------------------------

## 5. Service Layer (Business Logic)

Normally controllers call a **service layer**.

Example:

``` java
@Autowired
ContactService service;
```

Flow:

    Controller
       ↓
    Service

Service handles **business logic**, such as:

-   validation
-   business rules
-   processing data

------------------------------------------------------------------------

## 6. DAO Layer (Database Access)

Service communicates with the **DAO / Repository layer**.

    Service
       ↓
    DAO
       ↓
    Database

DAO responsibilities:

-   Run SQL queries
-   Interact with database
-   Fetch or save data

Examples:

-   save contact
-   get contact list
-   delete contact

------------------------------------------------------------------------

## 7. Returning the View

The controller returned:

    contact

Spring now uses a **ViewResolver**.

It converts:

    contact

into the actual page:

    /WEB-INF/views/contact.jsp

(or a Thymeleaf template depending on configuration)

------------------------------------------------------------------------

## 8. Response Back to Browser

Final flow:

    Browser
       ↓
    Tomcat
       ↓
    DispatcherServlet
       ↓
    ContactController
       ↓
    ViewResolver
       ↓
    contact.jsp rendered
       ↓
    Tomcat
       ↓
    Browser

The browser receives the **HTML response** and displays the page.

------------------------------------------------------------------------

## Complete Spring MVC Flow

    Browser
       ↓
    Tomcat (Servlet Container)
       ↓
    DispatcherServlet
       ↓
    HandlerMapping
       ↓
    Controller
       ↓
    Service
       ↓
    DAO
       ↓
    Database
       ↓
    Controller returns view
       ↓
    ViewResolver
       ↓
    JSP / HTML
       ↓
    Browser

------------------------------------------------------------------------

### One-Line Summary

**DispatcherServlet receives the request, finds the correct controller,
executes business logic through service and DAO layers, resolves the
view, and returns the response to the browser.**