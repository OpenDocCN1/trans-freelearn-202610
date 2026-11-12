
## Car Check-In

PPA wants to automate its processes as much as possible. The check-in process should look like the following:

- 1. Each office has multiple tablets with a particular application to allow customers to check in using the activation code they received during the reservation. Once the code is entered, the application should validate the code, booking location, and date. If the validation is successful, a confirmation message will be displayed on the screen, and the reservation record is updated to indicate the customer has checked in for collection. All tablets are connected to the internet.
- 2. Once the customer checks in, a sales agent can hand over the car keys and update the reservation status to indicate that they collected the keys.

PPA has shared the following requirements for the car check-out process.

## Car Check-Out

The check-out process should look like the following:

- 1. When the customer returns the car, the sales agent should collect the keys and quickly inspect the vehicle. The sales agent should enter any notes related to changes in the car’s condition, such as dents or scratches. If major damage is detected, the office technicians should be informed and asked to further investigate the car’s condition.
- 2. This information should be sent to the ERP, which generates a customer invoice after 72 hours.
- 3. Once an invoice is generated, a notification would be sent to the client with the invoice amount and instructions to settle any remaining balance.
- 4. The sales agent updates the reservation status to closed once the inspection is done.

PPA has shared the following requirements for the car status update.

## Car Status Update and Penalty Settlement

As mentioned before, all cars are fitted with a GPS tracking device. The following details describe the car status update and penalty settlement process:

- All tracking devices can communicate over 4G and 3G. They send the car’s location every 30 seconds. For security reasons, this continues to happen even if the car is turned off.
- PPA would like to gather information that describes the behavior of drivers, including going over speed limits and harsh braking. That all should be analyzed for PPA to calculate driving patterns during specific dates and times of the year.
- PPA would like to use GPS tracking devices to detect possible vehicle theft. If the device is reporting a change in the car’s location while the car is turned off, this could be a possible theft incident that the support team should be notified about within no more than 15 minutes.
- Every day, the violation databases should be checked. If they contain a violation record for a PPA car on a given date, the system should look up the car’s driver details on the given date, then transfer the penalty amount to the ERP to calculate a new invoice and send it to the customer.

PPA has shared the following data migration requirements.

## Data Migration Requirements

Considering the previously shared information about the current landscape, PPA has shared the following data migration requirements:

- PPA would like to migrate the data from all its current customer and rental management applications to the new system.
- PPA is aware that the current data contains many redundancies and duplicates and is looking for your guidance to deal with the situation and achieve high data quality in the target system.
- PPA has around 50 million rental records in each of its current rental management applications. Most of them are stored for historical purposes as PPA wants to keep rental records for at least 10 years. PPA would like to continue allowing its sales and support agents to access these records in the new system.

PPA has shared the following accessibility requirements.

## Accessibility and Security Requirements

PPA is looking for guidance to design a secure solution; they shared the following requirements:

- Customer accounts should be visible to office managers, sales agents, and service agents worldwide. However, technicians should only see customers from their specific country.
- Support incidents should be visible to support agents and their managers only.
- Only office technicians can update the car status to indicate that it is out of service and requires repairs.
- Office technicians should not be able to view the customer’s driving license number.
- Once a theft incident is detected, only the support agents trained for such incidents, their managers, and regional executives can view the incident’s details.
- The regional executive team should have access to all regional data, including drivers’ behavior, such as harsh braking.
- Office managers should be able to see the details of the partner accounts in their own country only.
- Partners should be able to log in to Salesforce using Salesforce-managed credentials only.
- Customers can self-register on the online portal and mobile application.
- Customers should also be able to log in to the online portal and the mobile application using their Facebook accounts.
- PPA employees who are logged in to the corporate network should be able to automatically log in to Salesforce without the need to provide their credentials again.
- PPA employees can log in to Salesforce from outside the corporate network using the LDAP credentials. However, if they log in from outside the network, they should provide a second factor of authentication, such as a text message received on their mobile phone.

PPA has shared the following reporting requirements.

## Reporting Requirements

PPA has requested a rich set of reporting capabilities, including the following:

- The regional executive team would like a report that shows relations between customers’ driving behavior and particular times of the year across the past 10 years.
- The regional executive team would like a trending monthly report that shows the pickup and dropoff locations of each rental to determine the appropriate distribution of cars across their offices.
- Customers should be able to view a full history of their rental bookings, including bookings from the past three years.
- Customers should also be able to view their full trip history for each completed rental.
- Partners should be able to view a monthly report showing the repair jobs completed for PPA.

PPA has shared the following project development requirements.

## Project Development Requirements

Considering the complexity of PPA’s program, they have the following project development requirements:

- PPA plans to have three different service implementers (SIs), delivering different functionalities across parallel projects. PPA would like to avoid conflicts across the teams, such as code overwriting or breaking each other’s logic. PPA requires your help to define a way to manage the code base to avoid such conflicts and reduce the chances of breaking existing functionalities.
- Historically, SIs used to follow their own coding standards while dealing with other platforms. On many occasions, they introduced duplicate functionalities. PPA would like to understand how to control and avoid this situation in Salesforce and enable better code reuse.
- PPA has a new platform owner who joined from another company that also used Salesforce. They reported that on several occasions, bugs were fixed in UAT but then showed up again in production. PPA would like to understand the likely cause of such an incident and how to avoid it in their program.
- PPA would like to release the solution to Italy first, then roll out the same solution to other countries based on a pre-agreed schedule. Each region country may have country-specific requirements that need to be defined and accommodated in the solution. PPA is looking for your support to determine the right development methodology and governance to achieve that.

PPA has also shared the following other requirements.

## Other Requirements

PPA would like to know the best practices to manage their customer consent. They have shared the following requirement:

PPA wants to ensure that data in development environments is always anonymized.

This concludes the hypothetical scenario. Ensure you have gone through all the pages and requirements of your hypothetical scenario before proceeding further.

Next, it is time to start identifying requirements and creating a draft solution following the same methodology you used to solve the domain-specific mock scenarios in Chapters 5-11.

## Analyzing the Requirements and Creating a Draft End-To-End Solution

Give yourself enough time to go through the scenario and understand the big picture. You can even start adding notes of potential solutions. If you plan to do the in-person review board, you can make the notes on a separate paper or on the printed scenario itself, while you can use Excel sheets for the virtual review board. You can use a combination of the two for both experiences; it depends on your preference and what you feel comfortable with.
