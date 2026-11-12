Now that you have solved the first business process, move on to the second.

# Car Reservation

PPA has shared three modes of the car reservation process: the first is via the online services (the online portal and the mobile application), the second is via the call center, and the third is via a walk-in visit to a PPA branch.

Start with the first requirement of the car reservation process.

The customer should be able to enter the pickup location (city) and start and end dates.

The system should show a list of cars available at that location and point in time. This will require a custom Visualforce page or Lightning component. The page would query the available cars based on the given parameters and display the results. Are you missing anything here? Actually, yes.

You need to think of the end-to-end solution. It is your responsibility as the lead architect of this project. One of the biggest mistakes a candidate could commit is to assume that someone else would take care of it. You need to consider the following challenges:

+   - Which object is going to be used to represent a car?
+   - How do you maintain the availability of cars?
+   - How are you going to organize the cars per location?
+   - What are the accessibility settings that will allow customers to view cars?

> Note
> 
> It is a common misunderstanding to assume that the way to prepare for the CTA review board is via engaging with more pre-sales activities. Pre-sales activities would significantly enhance your communication skills, but that alone is not enough.
> 
> While engaged in pre-sales activities, you rarely dive into the level of detail you encounter in this scenario. That level of detail is a must to pass the CTA review board. You need to have the breadth of knowledge, the consultant mindset, the hard-core technical depth, and a wealth of soft skills. Combining pre-sales activities with hard-core day-to-day implementation work would significantly boost your learning curve.

Can you use any standard object to represent the cars? The Product object is an obvious choice considering that it is also related to the Order object that you might use for the car reservation. You can utilize the PriceBook object to group cars by location.

The pricing is going to be handled by PPA's global calculator application anyway (you will learn more about this in the next requirement). You can also use a custom object, but you might need to make some adjustments to the Order object in that case.