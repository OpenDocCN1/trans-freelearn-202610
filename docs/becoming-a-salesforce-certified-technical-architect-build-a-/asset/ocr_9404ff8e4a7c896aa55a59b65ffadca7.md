# Car Check-In

PPA has shared two requirements for this process.

*Each office has multiple tablets with a particular application to allow customers to check in using the activation code they received during the reservation.*

The customer is expected to interact with the tablets in the office by entering the activation code. The application installed on the tablet should authenticate to Salesforce, look up the order record using the activation code, then update its status (or sub-status) to indicate the customer has checked in. There will probably be validation in place to prevent early check-in.

In your solution, you need to explain the integration pattern used (remote call-in), the authentication mechanism (OpenID Connect User-Agent Flow), and the security measurements to protect data in transit.

> **Note**
> 
> Authentication on its own does not protect data in transit (mutual authentication is an exception). Most authentication standards require a secure HTTPS connection. However, this is not mandatory for simple authentication. If a simple authentication took place over a non-secure HTTP connection, there would be no security measures to protect the data in transit. Your data will be transferred in plaintext format, and attackers can sniff the network traffic and get access to your data. The most common way to protect data in transit is using one-way or two-way TLS.

Take a moment to think of the potential changes that could be introduced to this requirement during the Q&A. How can you enhance this process? A possible addition could be to send a push notification to the sales agents in the current office shift to inform them of the check-in. This will make the process more streamlined. This is not a shared requirement at the moment but be prepared for such additions/changes during the Q&A.

Update your landscape architecture diagram, your list of integration interfaces, and your data model adding any new fields required, such as a sub-status), then move on to the next requirement.

*Once the customer checks in, a sales agent can hand over the car keys and update the reservation status to indicate that they collected the keys.*

You can assume that this process will happen manually. The agent will search for the reservation, validate the order’s sub-status, then hand over the keys and manually update the order again. Further automation could be requested during the Q&A. So, be prepared for that.