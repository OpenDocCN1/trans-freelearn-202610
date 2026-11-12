
I have identified the following six actors. They will need different types of licenses based on their expected roles. For example, the office managers and sales agents [point to them on the diagram] will need a Sales Cloud license because they need to review bookings and inspect returned cars. They need to create Cases, which, in my proposal, represent the inspection reports. They could utilize either the Sales Cloud or Service Cloud licenses, but I assumed the Sales Cloud license considering their role and the potential need to deal with leads.

The office technicians will utilize a Service Cloud license as they need to work with the Case object during the inspection process. This is not something supported by the Platform License. Support agents would also utilize the Service Cloud license as they need to work with different types of Cases.

Regional executives will need Service Cloud and Sales Cloud licenses as they will need access to all the data in the org. They will also need a CRM Analytics license as they will use this tool for trending reports and some other advanced analytics.

Finally, we have customers and partners. The former will be utilizing a Salesforce Customer Community Login license, while the latter will use a Salesforce Partner Community license. The Customer Community license will be a good fit for the one million PPA customers, considering that it is tailored for a vast number of users. Moreover, I am confident we can manage their accessibility requirements using functionalities such as sharing sets.

PPA partners will need a Partner Community license because they need to access PriceBooks, Products, Orders, Cases, and Reports. Moreover, we need sharing rules to control their record visibility. Furthermore, I assumed they will need access to leads, which is a typical requirement for partners.

That concludes the overall high-level solution presentation. This part should take no more than five minutes to cover from end to end.

Next, move on to the most significant part of your presentation, where you will explain how your solution will solve the shared business processes.

## Presenting the Business Processes' End-to-End Solution

You are still in the structured part of your presentation. For each business process, you will use the relevant business process diagram you created. In addition, you will be using the landscape architecture diagram, the data model, and the list of interfaces to explain your solution's details. You can start this part with a short intro, such as the following:

PPA shared five business processes. I will go through each and explain how the solution fulfills it.

Next, you will go through a sample presentation for the first business process.

### Customer Registration

You have already planned a solution for this process and created a business process diagram. Check Figure 12.4: Customer registration business process diagram. You will use this diagram to tell the story and explain this process's end-to-end solution. Your pitch could be as follows:

I will start with customer registration. This process can be triggered via two channels, either online or via the call center. Typically, PPA would be looking for a unified experience across channels.

I propose using Salesforce communities to develop a customer portal. I will configure the Customer Community to allow users to self-register. When the customer attempts to access the community, they will be directed to a login page with a link at the bottom of it to register a new account. When that link is clicked, the user will be redirected to the registration page.

The registration page will contain a Lightning component with input fields such as first name, last name, email address, password, and driving license number. Moreover, the client will be shown a section to provide consent for the privacy policy. If all mandatory fields are populated, the user can submit the page.

When the submit button is clicked, I will initiate a synchronous blocking web service call to a REST web service exposed by MuleSoft and pass the driving license number. MuleSoft will, in turn, call the right national driver and vehicle licensing agency and get back the result.

If the received result is positive, the Lightning component will create a Person account and a Community user for the customer. The Person account will be owned by a user outside the role hierarchy to avoid ownership skew. We can use a nightly batch job to assign this account to another user at a later stage.

The Lightning component will also create Individual and Contact Point Type Consent records to hold the captured consent. I am aware that we will need to use Future methods or Queueable Apex to avoid the mixed DML exception. Once the Community User is created, the customer will receive a notification email confirming the registration and can log in using the password set.

The same experience will also be provided to the call center's support agents. However, they will be using a different UI. The support agents will use the standard Salesforce UI to create the Person account, Individual, and Contact Point Type Consent records. Once all the data is in place, the agent will click on a button to validate the driving license. This button will invoke the exact same synchronous interface described earlier. Once the positive result is returned, the agent can proceed with creating the Community user.

The integration interface is described with all the details in my list of interfaces.
It will follow the Remote process invocation – request and reply pattern.
Authentication will be achieved using the OAuth 2.0 web server flow followed by
the Refresh Token flow. The data will be protected in transit using two-way TLS
between Salesforce and MuleSoft and one-way TLS between MuleSoft and the
national agency systems.

You should be pointing to the right integration interface on your list, the objects in your data model,
and the other different elements in your landscape architecture diagram, such as MuleSoft and the
national driver and vehicle licensing agency systems.

You can be a bit creative with your presentation and bring forward a requirement from the accessibility
and security requirement section. You could add the following:

Later on, there is a requirement to allow users to sign in using their Facebook
credentials. To enable that, I will define a new authentication provider in
Salesforce. I will set that to Facebook and define a registration handler class. I
will need the registration handler to determine whether the user already exists or
whether there is a need to provision it. Once I finish setting up the authentication
provider, I will configure my community to use it on its login page.

Once that is done, a new button will show on the community's login page with the
title Login with Facebook. When the user clicks the button, an OpenID
Connect web server authentication flow will be initiated. The user will be redirected
to Facebook to authenticate. Once that is done, a screen will display a message
to the user asking for permission to access personal data. The page will have
approve and reject buttons. Once the user clicks on the approve button, they will be
redirected back to the Salesforce Community.

The registration handler will receive a payload containing data such as first name,
last name, and email address. It will use that to determine whether the user
already exists or needs to be created. If the user already exists, the user will be
automatically logged in to the community. Otherwise, the registration handler will
create the user.

However, before the user can proceed further, we need to capture the driving
license. This is not a value that we will receive from Facebook. Therefore, we need
to use a Custom Login Flow to prevent Community users with no value in their
driving license field from logging in to the community, and instead display a
custom VisualForce page that captures the driving license.

The page will have a button. Once the button is clicked, it will invoke the validation
web service explained earlier to validate the driving license and return the result. If
the result is positive, the user will be allowed into the community.

You have seeded some questions, such as the full description of the OAuth 2.0 web server flow. If the
judges feel a need to get more details, they will come back to this point during the Q&A.
