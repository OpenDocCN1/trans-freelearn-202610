+   4. Scaffold GWT application using the `gwt setup` command:
+   
+   `... roo> gwt setup`

+   5. If you want to import `flightapp-gae-gwt` into Eclipse IDE , execute the `perform eclipse` command:
+   
+   `... roo> perform eclipse`

+   6. Exit the Roo shell and execute the `gae:run` goal of Maven GAE Plugin to run the `flightapp-gae-gwt` application locally on the Google App Engine development web server that comes bundled with App Engine SDK for Java, as shown here:
+   
+   `C:\roo-cookbook\ch06-gae-gwt> mvn gae:run`

+   7. The Maven GAE Plugin was configured in the `pom.xml` file of the `flightapp-gae-gwt` project when we executed the `persistence setup` command. A successful start of development server will show the following message: The server is running at `http://localhost:8080/`

+   8. Now, open your favorite web browser and go to `http://localhost:8080` to access the GWT `flightapp-gae-gwt` web application, which allows you to perform CRUD operations on the `FlightDescription` JPA entity, as shown here:

![](bbox=[0.215, 0.45, 0.785, 0.655])