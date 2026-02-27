# JSP Elements (Core)

## 1️⃣ Directive

* Used to give instructions to the JSP container.

    ```jsp
    <%@ page import="java.util.Date" %>
    <%@ include file="header.jsp" %>
    <%@ taglib uri="..." prefix="c" %>
    ```

* **Types:**
    - page
    - include
    - taglib

## 2️⃣ Declaration

* Used to declare variables/methods outside service() 
* Becomes class-level in servlet.

    ```jsp
    <%! int count = 0; %>
    <%! 
        public int add(int a, int b){ 
            return a+b; 
        } 
    %>
    ```

## 3️⃣ Scriptlet

* Java code inside service().

    ```jsp
    <%
        int x = 10;
        out.println(x);
    %>
    ```

## 4️⃣ Expression

* Prints output directly to response.

    ```jsp
    <%= new Date() %>
    ```
<br>

## Action Tags

* Used for runtime behavior.
- `<jsp:include page="file.jsp" />`
- `<jsp:forward page="home.jsp" />`
- `<jsp:useBean id="obj" class="com.test.Student" />`

## Implicit Objects
* Predefined objects:
    | Object      |
    |-------------|
    | request     |
    | response    |
    | session     |
    | application |
    | out         |
    | config      |
    | pageContext |
    | page        |
    | exception   |