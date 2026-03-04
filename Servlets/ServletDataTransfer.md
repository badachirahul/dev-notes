# Servlet Data Transfer Mechanisms

## **1. RequestDispatcher (Forward)**
* 
    ### What It Is

    Used to forward a request from one servlet to another resource
    (servlet/JSP) within the SAME request.

    ### How It Works

    -   Same request and response objects are shared.
    -   Server internally transfers control.
    -   Browser URL does NOT change.

    ### Sending Data

    ``` java
    request.setAttribute("data", 100);
    RequestDispatcher rd = request.getRequestDispatcher("second");
    rd.forward(request, response);
    ```

    ### Accessing Data in Second Servlet

    ``` java
    Integer value = (Integer) request.getAttribute("data");
    response.getWriter().println(value);
    ```

    ### When to Use (Best For)

    -   ✔ Passing data within same request
    -   ✔ MVC pattern
    -   ✔ Fast internal communication

    ### Is It Better?
    -   ✅ YES --- Best when you want to transfer data internally without
        exposing it in URL.

<br>

## **2. sendRedirect (URL Rewriting)**
* 
    ### What It Is

    Tells the browser to make a NEW request to another URL.

    ### How It Works

    -   Server sends redirect response.
    -   Browser makes new request.
    -   URL changes.

    ### Sending Data

    ``` java
    response.sendRedirect("second?data=100");
    ```

    ### Accessing Data in Second Servlet

    ``` java
    String value = request.getParameter("data");
    response.getWriter().println(value);
    ```

    ### When to Use

    -   ✔ Redirect after form submission (PRG pattern)
    -   ✔ Redirect to another domain

    ### Is It Better?

    -   ❌ Not better for data sharing
    -   ✔ Better only when you want URL change or external redirect

<br>

------------------------------------------------------------------------

## **3. HttpSession (Session Scope)**
* 
    ### What It Is

    Stores user-specific data across multiple requests.

    ### How It Works

    -   Server creates session.
    -   Session ID stored in cookie (JSESSIONID).
    -   Data lives until logout or timeout.

    ### Sending Data

    ``` java
    HttpSession session = request.getSession();
    session.setAttribute("data", 100);
    response.sendRedirect("second");
    ```

    ### Accessing Data in Second Servlet

    ``` java
    HttpSession session = request.getSession(false);
    Integer value = (Integer) session.getAttribute("data");
    response.getWriter().println(value);
    ```

    ### When to Use

    -   ✔ Login systems
    -   ✔ Shopping cart
    -   ✔ Multi-step forms

    ### Is It Better?

    -   ✅ Best for user-specific persistent data
    -   ❌ Not ideal for small one-time transfers (uses server memory)

<br>

------------------------------------------------------------------------

## **4. ServletContext (Application Scope)**
* 
    ### What It Is

    Shared data across entire application (all users).

    ### Sending Data

    ``` java
    ServletContext context = getServletContext();
    context.setAttribute("data", 100);
    ```

    ### Accessing in Another Servlet

    ``` java
    ServletContext context = getServletContext();
    Integer value = (Integer) context.getAttribute("data");
    response.getWriter().println(value);
    ```

    ### When to Use

    -   ✔ Global configuration
    -   ✔ Shared counters
    -   ✔ Application-wide constants

    ### Is It Better?

    -   ❌ Not for user-specific data
    -   ✔ Good for global shared data

    ------------------------------------------------------------------------

    ## Which One Is Better?

    Situation               Best Choice
    ----------------------- -------------------
    Same request transfer   RequestDispatcher
    Redirect after POST     sendRedirect
    Login / user data       HttpSession
    Global shared data      ServletContext

    ------------------------------------------------------------------------

    ## Interview Final Summary

    -   Forward = Same request, internal, secure
    -   Redirect = New request, URL visible
    -   Session = Per-user storage
    -   ServletContext = Application-wide storage

    Understanding these clearly means you understand how Spring MVC handles
    Model, Session, and Application scopes internally.
