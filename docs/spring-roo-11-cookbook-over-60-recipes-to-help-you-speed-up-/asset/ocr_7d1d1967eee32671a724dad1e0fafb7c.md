Start the Roo shell from the ch05-webflow directory and execute the ch05_webflow_app.roo script using the script command. Executing the ch05_webflow_app.roo script creates a flightapp-webflow Eclipse project, sets up Hibernate as persistence provider, configures MySQL as the database for the application, and creates Flight, FlightDescription, Customer, and Address JPA entities. The Customer entity has a one-to-one relationship with the Address entity and Flight has a many-to-one relationship with the FlightDescription entity. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

# How to do it...

To set up the Spring Web Flow framework, follow the steps given here:

+   1. Execute the web flow Roo command to create a Customer flow, as shown here:
+   .. roo> web flow --flowName customer
+   Created SRC_MAIN_WEBAPP\WEB-INF\spring
+   Created SRC_MAIN_WEBAPP\WEB-INF\spring\webflow-config.xml
+   Created SRC_MAIN_WEBAPP\WEB-INF\spring\webmvc-config.xml
+   ...
+   Created SRC_MAIN_WEBAPP\WEB-INF\views\customer
+   Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\flow.xml
+   Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\view-state-1.jspx
+   Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\view-state-2.jspx
+   Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\end-state.jspx
+   Created SRC_MAIN_WEBAPP\WEB-INF\views\menu.jspx
+   Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\views.xml
+   ...
+   Updated ROOT\pom.xml [...; added dependency org.springframework.webflow:spring-webflow:w:2.2.1.RELEASE]

+   2. Execute the perform eclipse command to update the project's classpath settings:
+   .. roo> perform eclipse

+   3. You can now import the flightapp_webflow project into your Eclipse IDE.

+   4. Exit the Roo shell and execute the tomcat:run goal of the Tomcat Maven plugin from the ch05-webflow directory to deploy the flightapp_webflow project to the embedded Tomcat container:
+   C:\roo-cookbook\ch05-webflow> mvn tomcat:run