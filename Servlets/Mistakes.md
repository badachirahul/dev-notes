# Servlet Learning -- Mistakes & Corrections Notes

------------------------------------------------------------------------

# 1. GET vs POST Mismatch
* 
    ## Mistake

    HTML form did not specify method="post" but servlet had doPost().

    ## What Happened

    Servlet method was never executed because default form method is GET.

    ## Correction

    Make sure form method matches servlet method.

    Example:

    ``` html
    <form action="add" method="post">
    ```

------------------------------------------------------------------------

# 2. Forgot Redirect / Forward After Storing Data
* 
    ## Mistake

    Stored data in session or cookie but did not redirect or forward to
    another servlet.

    Example mistake:

    ``` java
    session.setAttribute("data", result);
    ```

    ## What Happened

    Next servlet was never triggered.

    ## Correction

    Add redirect or forward:

    ``` java
    response.sendRedirect("square");
    ```

------------------------------------------------------------------------

# 3. NullPointerException (Calling equals on Null)
* 
    ## Mistake

    ``` java
    checkName.equals("admin");
    ```

    If checkName is null → crash.

    ## What Happened

    HTTP 500 -- Internal Server Error.

    ## Correction

    Always compare like this:

    ``` java
    "admin".equals(checkName);
    ```

------------------------------------------------------------------------

# 4. Incorrect request.getParameter Usage
* 
    ## Mistake

    ``` java
    request.getParameter(username);
    ```

    username treated as variable, not string literal.

    ## Correction

    Always pass parameter name in quotes:

    ``` java
    request.getParameter("username");
    ```

    Must match HTML input name exactly.

------------------------------------------------------------------------

# 5. Cookie Created but Not Used Properly
* 
    ## Mistake

    Created cookie but did not redirect.

    ``` java
    response.addCookie(cookie);
    ```

    ## What Happened

    Cookie not available in same request.

    ## Correction

    Cookie works only in next request. Add redirect:

    ``` java
    response.sendRedirect("nextServlet");
    ```

------------------------------------------------------------------------

# 6. No Null Check on Session Attribute
* 
    ## Mistake

    ``` java
    int n = (int) session.getAttribute("data");
    ```

    If attribute is null → NullPointerException.

    ## Correction

    Use safe casting:

    ``` java
    Integer n = (Integer) session.getAttribute("data");
    if (n != null) {
        // use n
    }
    ```

------------------------------------------------------------------------

# 7. Confusion Between Request, Session, and Cookie Scope
* 
    ## Mistake

    Mixing up when data becomes available.

    ## Correct Understanding

    -   Request: Works only in same request (requires forward)
    -   Session: Works in same and future requests
    -   Cookie: Available only in next request (after browser sends it back)

------------------------------------------------------------------------

# Final Learning Summary

Most mistakes were not logic errors but:

-   Request lifecycle confusion
-   Scope misunderstanding
-   Null handling issues
-   Small syntax carelessness

These are common beginner mistakes in Servlets and are part of the
learning process.