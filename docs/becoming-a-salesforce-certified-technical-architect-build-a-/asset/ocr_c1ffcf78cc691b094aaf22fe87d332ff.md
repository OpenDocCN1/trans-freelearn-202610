# Car Check-In

PPA wants to automate its processes as much as possible. The check-in process should look like the following:

+   1. Each office has multiple tablets with a particular application to allow customers to check in using the activation code they received during the reservation. Once the code is entered, the application should validate the code, booking location, and date. If the validation is successful, a confirmation message will be displayed on the screen, and the reservation record is updated to indicate the customer has checked in for collection. All tablets are connected to the internet.
+   
+   2. Once the customer checks in, a sales agent can hand over the car keys and update the reservation status to indicate that they collected the keys.

PPA has shared the following requirements for the car check-out process.

# Car Check-Out

The check-out process should look like the following:

+   1. When the customer returns the car, the sales agent should collect the keys and quickly inspect the vehicle. The sales agent should enter any notes related to changes in the car’s condition, such as dents or scratches. If major damage is detected, the office technicians should be informed and asked to further investigate the car’s condition.
+   
+   2. This information should be sent to the ERP, which generates a customer invoice after 72 hours.
+   
+   3. Once an invoice is generated, a notification would be sent to the client with the invoice amount and instructions to settle any remaining balance.
+   
+   4. The sales agent updates the reservation status to closed once the inspection is done.

PPA has shared the following requirements for the car status update.

# Car Status Update and Penalty Settlement

As mentioned before, all cars are fitted with a GPS tracking device. The following details describe the car status update and penalty settlement process:

+   • All tracking devices can communicate over 4G and 3G. They send the car’s location every 30 seconds. For security reasons, this continues to happen even if the car is turned off.
+   
+   • PPA would like to gather information that describes the behavior of drivers, including going over speed limits and harsh braking. That all should be analyzed for PPA to calculate driving patterns during specific dates and times of the year.