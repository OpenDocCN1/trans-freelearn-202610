# Car Check-Out

The process of returning the car to the leasing office is more straightforward. It can only be done in person by the client. Have a look at the first requirement, as follows:

When the customer returns the car, the sales agent should collect the keys and quickly inspect the vehicle.

The sales agent can look up the order related to this particular rental using the activation code or customer information. But where can you store the inspection details? And how can you inform the technicians about major damage to follow up with?

This is where you need to demonstrate your understanding of the platform’s capabilities. You can propose a solution based on a set of custom fields and workflow rules with email notifications. But then you will find yourself trying to solve other future challenges, such as the following:

+   - How can you send an email notification to an entire team? You are strongly advised not to propose any solution that is based on a shared email inbox pattern. That pattern was acceptable a few decades ago, but today’s enterprises want something far more traceable and auditable.
+   - How can you ensure that the technicians are acting on the incident? If they do not act, the right invoice will not be generated. This will impact the main process you are solving.
+   - If you are planning to introduce a set of new fields on the Order object, how could that impact the user experience? If you are planning to introduce a whole new custom object for the same purpose, then have you considered the standard objects that might deliver the desired functionality?

Considering that you are looking for the best solution technically, you should consider using the Case object whenever you detect a need for a well-governed incident management process, for requirements such as reporting, assigning, progress tracking, and eventually resolving an incident.

For this requirement, the sales agent can create a case (the Sales Cloud license permits that), link it with the customer’s order, and use it to provide structured feedback. If specific criteria are met, the case can be automatically assigned to the technicians’ queue for that office. Once the case is assigned to the queue, all queue members will be notified via an email alert (this feature can be turned on or off).

The queue assignment can also grant the technicians access and visibility to the case records. You could configure escalation rules to ensure that the case is reported to higher management if the technician took no action during a specified period.

This is a scalable and flexible solution. The judges can challenge you and change some requirements on the fly, but your solution is flexible enough to handle these changes.