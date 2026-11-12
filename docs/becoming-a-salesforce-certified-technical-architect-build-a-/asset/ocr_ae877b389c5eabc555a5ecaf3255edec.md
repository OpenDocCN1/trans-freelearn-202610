# # Scheduled Manual Meter Reading

PLU shared five bullet points to explain this business process (in the Scheduled Manual Meter Reading section). The first two bullet points provided information about the process and technology used, while the remaining three provided detailed requirements.

Go through these three bullet points and analyze and solution them, starting with the first requirement.

PLU would like the radar reader devices to be paired with the field service technician’s mobile phone, then use the phone to send the reading data to Salesforce.

The mobile app with the field service technician must be able to communicate with the radar reader via Bluetooth. This requirement should help you decide on your mobile strategy. This capability is unlikely to be supported by a hybrid app. So, a native app is required. But before you reach a final decision, have a look at the next two requirements.

PLU is looking to optimize the visiting time and travel costs for their field service technicians.

You have two out-of-the-box capabilities in Salesforce to optimize visiting time and travel costs, that is, Salesforce Maps and Salesforce Field Service. The latter has more advanced capabilities and is more geared toward field service requirements and activities, such as skills-based assignments, shift management, and crew management.

Select that to cover this requirement. You need to add the relevant Field Service objects to your data model. Make yourself familiar with the standard Field Service objects and be prepared to explain how they are used.

> > Note
> > You can find more details about Field Service objects at the following link: https://packt.link/K6cBm.

Add the WorkOrder and ServiceAppointment objects only to your data model to keep things simple.

Update your data model and landscape architecture, then move on to the next requirement.

At any given time, PLU would like to track the location of its field service technicians.

You know that Salesforce Field Service has a specific mobile app that offers this feature out of the box. This requirement can be fulfilled with that. But can you extend the Salesforce mobile application to communicate with the radar reader over Bluetooth?

You can change some configurations on the mobile app. Still, even if you do not know the app’s full limitations, it is risky to assume that it can be extended to cover such sophisticated requirements.