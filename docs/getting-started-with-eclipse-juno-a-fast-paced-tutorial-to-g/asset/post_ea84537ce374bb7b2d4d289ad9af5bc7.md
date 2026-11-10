
![](img/d7c3f2018603810fae83e1a6fd98749e_169_0.png)

We have not implemented the servlet responsible for forwarding the values and operation to the class that implements the calculator. Therefore, if you type two numbers in the 1st operand and 2nd operand fields, select an operation, and press Submit, you will get the page shown in the next screenshot. This indicates that the resource we are looking for was not found by the servlet container. So, let's take a step back and see why the servlet container is trying to forward the data to webcalculator/PerformCalculation.do.

The answer to this question boils down to the fact that our form is submitting information to something named PerformCalculation.do (take a look at the previous chunk of code). Further, given that there is no resource (servlet or HTML page, for instance) with such a name, the container shows an error message. In this example, PerformCalculation.do is a logical name. That is to say, it is not an actual filename. We do not want the client to have access to the names of our servlet classes. So, we use made-up names that are mapped to actual servlets instead. That explains why we got an error page: we have not informed the container yet about which servlet answers by the logical name PerformCalculation.do.

> The .do extension (which implies *do something*) is just a convention.

![](img/d7c3f2018603810fae83e1a6fd98749e_169_1.png)

Implementing the second part of our example entails mapping a servlet to the aforementioned made-up name. Again, use Eclipse to generate the servlet for you. Name it `ArithmeticExpert`, and place it into the same package of the previous servlet (`com.example.calculator.web`). When creating `ArithmeticExpert`, and after going through the first window where you type the servlet's name and package, click on **Next**, and then click on **Edit...** as shown in the following screenshot. Here you can edit the servlet URL mapping. Given that we want to inform the container that the new servlet can be mapped to `PerformCalculation.do`, edit this option as shown in the following code snippet (make sure you do not start with a / character) and press **Next**:

![](img/d7c3f2018603810fae83e1a6fd98749e_170_0.png)

Rather than overriding `doGet`, this time we will override `doPost`. Because the form in `OperationSelect` generates a `POST` operation, we have to override `doPost`. Otherwise, pressing the **Submit** button will not invoke our method. Therefore, unselect the **doGet** and **Constructors from superclass** options. Lastly, hit **Finish**. Change the generated code, as shown in the following snippet:

```
@WebServlet("/PerformCalculation.do")
public class ArithmeticExpert extends HttpServlet {
    private static final long serialVersionUID = 1L;
    @Override
    protected void doPost(HttpServletRequest request,
HttpServletResponse response)
    throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter pw = response.getWriter();
        pw.write("<html><body>");
        pw.write("The result of " +
        request.getParameter("firstOperand") + " " +
        request.getParameter("operation") + " " +
        request.getParameter("secondOperand") +
        " is surprise, surprise");
        pw.write("</body></html>");
        pw.close();
    }
}
```

We are implementing our calculator example in stages. In this first version, our goal is to make sure that the communication link between the first and second servlets is working properly. Therefore, our code does not invoke the calculator class to perform the selected operation. Instead, it will just print out a message indicating that the communication between both servlets is working and that we are able to share information between them. There are two important points to note in the preceding code. First, see how the mapping between the servlet and its made-up name, which appears in the output of OperationSelect, is implemented using the @WebServlet annotation. Note that the string enclosed in parentheses is the pattern we typed during the creation of the underlying servlet (check the previous screenshot). Second, note how we access the parameters from the request. We use the method getParameter from HttpServletRequest (actually, this method is defined in the interface ServletRequest, and implemented by subclasses including HttpServletRequest). Using getParameter, each parameter is retrieved by its name.

![](img/d7c3f2018603810fae83e1a6fd98749e_171_0.png)

Parameter names are case sensitive.

Upon running the previous servlet and typing 2 in the **1st operand** textfield, 1 in the **2nd operand** textfield, selecting subtraction as the operation to be performed, and pressing **Submit**, you should see something like the next screenshot. As you can see in the following screenshot, now the application can access data entered by the user in one of the servlets to perform the calculations in the other servlet:

![](img/d7c3f2018603810fae83e1a6fd98749e_172_0.png)

To finish the calculator example, we need to implement a calculator class that is able to perform the arithmetic operations shown in the first servlet. That should be straightforward for you now. Before checking our solution shown in the following snippet, try to write this class by yourself. Just make sure to name it `Calculator`, and place it into a package named `com.example.calculator.model`. The following is how we implemented Calculator:

```java
package com.example.calculator.model;
public class Calculator {
    private double op1, op2;
    private String operation;
    public Calculator(String op1, String op2, String operation) {
        try {
            this.op1 = Double.parseDouble(op1);
            this.op2 = Double.parseDouble(op2);
            this.operation = operation;
        }
        catch (NumberFormatException e) {
            //reset the value of both operands
            this.op1 = 0.0;
            this.op2 = 0.0;
        }
    }
    public double performCalculation() {
        if (operation.equals("+")) {
            return op1 + op2;
        } else if (operation.equals("-")) {
            return op1 - op2;
        } else if (operation.equals("/")) {
            return op1 / op2;
        } else if (operation.equals("*")) {
            return op1 * op2;
        }
        return 0;
    }
}
```

After implementing Calculator, you just need to integrate it to the ArithmeticExpert servlet, as shown in the following snippet. Note that our implementation does not deal with edge cases as division by zero. Upon executing the example again, you will see something similar to what is shown in the following screenshot:

```
@Override
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws
    ServletException, IOException {
    response.setContentType("text/html");
    PrintWriter pw = response.getWriter();
    pw.write("<html><body>");
    String operandOne = request.getParameter("firstOperand");
    String operandTwo = request.getParameter("secondOperand");
    String operation = request.getParameter("operation");
    pw.write("The result of " +
        operandOne + " " +
        operation + " " +
        operandTwo +
        " is " +
        new Calculator(operandOne, operandTwo,
            operation).performCalculation());
    pw.write("</body></html>");
    pw.close();
}
```

![](img/d7c3f2018603810fae83e1a6fd98749e_173_0.png)

## Implementing your very first JSP using Eclipse WTP

The previous sections got us started with Tomcat, servlets, and Eclipse WTP. As you saw, despite the benefits provided by Eclipse WTP (it generated all the structure of the web project for us and also took care of generating a significant chunk of code when we implemented the previous example servlet), embedding HTML into your Java code feels a little awkward. As we mentioned, JSPs were introduced to make the creation of dynamic content more straightforward and appealing. JSPs let you mix HTML and Java code in a better way than servlets. Rather than embedding HTML into Java code, JSP makes it possible to embed dynamic content into HTML. Recall from the previous sections that a JSP is an HTML page containing some special elements. The elements that are rendered into dynamic content resemble HTML elements. However, they are componentized Java programs. Besides these HTML-like elements, a JSP page can also include Java code (scriptlets).

It bears reminding you of the fact that although JSPs look like standard HTML pages with additional elements, they are compiled down to servlets. As we mentioned, at runtime, Tomcat compiles JSP pages down to servlets, compiles the servlets to bytecodes, and executes them eventually.

In this section we will demonstrate how to use Eclipse WTP to create JSPs. We will implement a JSP that shows the same message of the servlet example.

You do not need to create a new web application project; we will use the previous one. To get Eclipse to create a new JSP file for you, do the following: select the folder `WebContent` (Tomcat will look for JSPs in this folder), and right-click on **JSP File** in **New**. Name it `HelloWorldJSP.jsp` (as shown in the following screenshot), and then hit **Next**. Actually, you do not need to type the extension (`.jsp`), Eclipse will include it automatically:
