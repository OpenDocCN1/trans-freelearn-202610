Now, create a Booking instance using the **Create new Booking** menu option of the generated web application, as shown in the following screenshot:

![](bbox=[0.208, 0.189, 0.792, 0.313])

Once you have created a Booking instance, you are ready to create a new Flight instance and associate a Flight with the newly created Booking instance. To create a new Flight instance, select the **Create new Flight** instance menu option, which shows the form for creating Flight instances, as shown in the following figure:

![](bbox=[0.278, 0.406, 0.722, 0.553])

As the given screenshot shows, Spring Roo didn't scaffold an HTML element to select multiple Booking instances, to help create a one-to-many relationship between Flight and Booking entity instances. So, how do we create the relationship between entities in case the relationship is one-to-many? To use the scaffolded Spring Web MVC application to manage relationships between Booking and Flight entities, specify the @ManyToOne annotated field in the Booking entity (the many side of the relationship) to create a many-to-one relationship between the Booking and Flight entities. Now, you can create the Flight instances (without specifying the related Booking instances), and manage the relationship between the Booking and Flight instances from the form for creating the Booking (the many side of relationship) instances.

If you don't want to add the @ManyToOne annotated field to the Booking entity, you can modify the /WEB-INF/flights/create.jspx view (which displays the form for creating the Flight instances) to add a field, which shows a multi-select list box that displays the Booking instances. The following <field:simple> custom tag (installed by Roo) in create.jspx shows the message: **This relationship is managed from the Booking side** when you select the menu option to view the form for creating the Flight instances: