For this diagram, you will need to do the following:

+   • Show all systems involved in the landscape architecture (including third parties, such as AppExchange products or external systems). In real life, you may also want to extend the landscape architecture so that it includes the key functionalities that are delivered by each system. During the review board, adding all key functionalities might take more time than you can allocate.

+   • Include the systems that you are planning to completely/partially retire. Find a way to differentiate them from the other systems that you want to add or keep. Color coding is a good idea, but you can simply add a symbol next to the system to indicate its status. In real life, you are probably going to create multiple copies of this diagram, with each representing a snapshot in time.

+   • Show the proposed integration interfaces. You also need to include information such as the integration pattern, how you are planning to secure your channel, and the authentication standard used. You can add this information directly to the diagram, but this could make it look clumsy. Alternatively, you can use an interface reference/code on the diagram and create a supporting table with full details. Then, you can use them both during the review board to explain your integration strategy.

+   • Remember to include SSO interfaces. You can also include any mobile devices that are part of your landscape. Add the type of the mobile app (Salesforce mobile, native custom app, hybrid app, or HTML5-based) right next to that.

Now, look at an example. Figure 1.5 shows the systems involved in a landscape where Salesforce is replacing a legacy CRM. You will also notice that it is integrated with external systems through some integration middleware (MuleSoft):