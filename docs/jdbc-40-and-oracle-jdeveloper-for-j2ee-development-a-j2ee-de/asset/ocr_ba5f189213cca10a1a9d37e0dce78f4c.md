In this section, a business components application module was created and tested with the Oracle database. In the next section, we will create a web application for the business components.

# Developing a BC4J Web Application

In this section a web application is created in the ViewController project of the Model-ViewController application. A web application JSP will invoke the business components in the Model project. Add the Model project business components to the classpath of the ViewController project. Select the ViewController project in the Applications Navigator. Select Tools | Project Properties in the JDeveloper IDE. In the Project Properties window, select the Common | Dependencies node. In the Project Dependencies window, click in the Model.jpr checkbox. Click on the OK button. The Model project gets added to the classpath of the View project.

![](bbox=[0.225, 0.39, 0.775, 0.68])

The JSP application will be integrated with the BC4J components in the Model project. To create a JSP right-click on the ViewController project and select New. In the New Gallery window, select Web Tier | Java Server Pages. Select JSP Page in the Items listed. In the Create JSP page, specify TestClient as the File Name and a Directory Name for the JSP page. The JSP page TestClient.jsp is added to the ViewController project of the BC4J application workspace. JDeveloper provides integrated support for the Oracle JDBC driver. For ADF/BC4J applications with a database other than Oracle database copy JDBC JAR file to BC4J/lib directory.