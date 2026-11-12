+   4. Remove the @NotNull JSR 303 annotation from the price field and save the FlightDescription.java file. Now, Roo shell shows the following output in response to the change:
+   
+   Updated... \WEB-INF\views\flightdescriptions\create.jspx
+   Updated... \WEB-INF\views\flightdescriptions\update.jspx

# How it works...

Let's now look at what Spring Roo did when we made modifications to Flight Description.java. The following adjustments were made by Spring Roo in the flight-app project when the finders element was removed:

+   Spring Roo removed the FlightDescription_Roo_Finder.aj ITD, which contained the implementation of the finder method.

> If a JPA entity defines multiple finder methods, then removing a single finder method from the finders attribute of the @ RooEntity annotation will only remove the corresponding finder method from the ITD file.

+   When the FlightDescriptionController_Roo_Controller_Finder .aj ITD was created initially, it contained a method for showing the form (refer to /WEB-INF/views/flightdescriptions/findFlightDescriptions ByDestinationAndOrigin.jspx), which allows searching for a FlightDescription based on origin and destination cities. It also contained a method to search the FlightDescription instances and display results (refer to /WEB-INF/views/flightdescriptions/list.jspx). Now, as the finder method has been removed, the controller methods to show the search form and search results are removed. It is important to note that in Spring Roo 1.1.3, the method responsible for searching entity instances is not created in FlightDescriptionController_Roo_Controller_Finder.aj. This bug is resolved in Spring Roo 1.1.4 and above.

> It is important to note that even though the findFlight DescriptionsByDestinationAndOrigin. jspx file is no longer required in the flight-app application, Spring Roo doesn't remove it. You will need to manually remove the JSPX file from your Roo project. This is because Spring Roo doesn't automatically delete JSPX files that are no longer required in the application.

+   As the finder method has been removed, the menu option Find by Destination and City is also removed from the menu.jspx file.