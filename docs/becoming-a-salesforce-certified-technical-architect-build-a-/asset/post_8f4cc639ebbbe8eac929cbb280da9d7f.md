
The service agents will utilize a Service Cloud license as they need to work with the Case object (which represents incidents and queries). Cases will be assigned to the right agent based on the required skills. Salesforce Omni-Channel will be used for that, which also explains the need for the Service Cloud license. The service agents will also use the Service Console and Salesforce Service Cloud Voice to answer support calls.

The SVPs will be using licenses that grant them access to all objects the teams have access to. They would also need a CRM Analytics license to access some trending reports that rely on data that does not exist in Salesforce standard or custom objects.

The Key customer managers will use a Sales Cloud license as they need to use the Opportunity object to manage deals with key customers.

The marketing team will require both Sales Cloud and Marketing Cloud licenses to create and nurture leads across both systems. Moreover, they will be executing marketing campaigns and sending mass emails using Marketing Cloud.

The Field Service team will use Service Cloud and Salesforce Field Service licenses. They need access to the Salesforce Field Service capabilities, such as the mobile application, the Field Service data model, and the optimized scheduling capability.

Maintenance partners will be using the Partner Community license. We do not have many details on the tasks they do apart from handling energy supply incidents. I assumed they would need some advanced sharing capabilities and visibility of some opportunities.

Finally, customers will be using a Salesforce Customer Community Login license. It will be a good fit considering that the company has nearly six million customers. This license is tailored for a vast number of users. We can manage their accessibility requirements using functionalities such as sharing sets.

That concludes the overall high-level solution presentation. This part should take five to six minutes to cover from the end. You can use your own words, of course, but you need to adjust your speed to ensure that you cover all the required information in the given time. Keep practicing until you manage to do that. Time management is crucial for this exam.

Now, move on to the most significant part of your presentation, where you will explain how your solution will solve the shared business processes.

## Presenting the Business Processes' End-to-End Solution

You are still in the structured part of your presentation. If you have created a business process diagram for a business process, then this is the time to use it. Besides, you will typically use the landscape architecture diagram, the data model, and the list of interfaces to explain your end-to-end solution. You can start this part with a short intro, such as the following:

*The company shared six business processes. I will go through each and explain how the solution fulfills it.*

You will create a sample presentation for the customer service business process.

## Customer Service

Similar to what you did in Chapter 13, Present and Defend: First Mock, you will try to tell a story that explains how their process will be realized using the new system. Your pitch could be as follows:

*I will now explain how the proposed solution will fulfill the customer service process requirements.*

*The customer can raise inquiries and complaints using an online portal or by calling the call center number. The inquiries and complaints will be modeled using the standard Case object. I propose using Salesforce Omni-Channel to assign the Case record to the right agent based on the required skills, such as language. There will be a need to evaluate multiple skills. Therefore, I decided to use Omni-Channel.*

*Salesforce communities will be used to create an online portal. As mentioned before, customers will be utilizing the Customer Community Login license.*

*Salesforce Service Cloud Voice can be used to set up a phone number for the call center. Service Cloud Voice will also integrate with the Service Cloud Console and provide a softphone that the agents can use to pick up calls and create cases.*

*I will use the case escalation rules to escalate complaints that are not resolved in seven days. The SVP of the service will receive an email once a complaint is escalated.*

*One of the shared requirements indicates that the system should automatically generate a forecasted meter reading for the next month based on the previous month’s reading. Considering the number of customers and meters we have in the system and considering that we have two figures to capture each month per meter (which are the forecasted and actual consumption), I estimated a need for around 146 million records to hold this data.*

*This number is enormous and will directly impact the performance of the target object’s CRUD operations. It will also consume a considerable amount of data storage and could have other performance impacts based on lookup skew.*

I will explain all identified LDVs and how I am planning to mitigate them later.

The meter readings will be stored in a custom denormalized object. This will significantly reduce the number of records needed to capture the monthly forecasted and actual consumption [point to the Meter_Reading__c object on your data model diagram].

I will use a before-save flow to populate the next month's forecasted consumption once the actual consumption for the previous month is captured. For example, the flow will automatically populate the Feb_Forecasted__c field's value once the Jan_Actual__c field is updated.

The Meter_Reading__c object will be linked to the Asset object using a master-detail relationship. This way, any user who has access to the Asset record will be able to view its related Meter_Reading__c. Customer Community users will get access to the assets related to the Account records they have access to; therefore, they will be able to view their relevant Meter_Reading__c records.

I have assumed that invoices and payments are stored in the ERPs. I propose exposing them to customers in Salesforce using Salesforce Connect. MuleSoft connects to all the ERPs, retrieves the invoices, and aggregates and exposes them as an OData interface that Salesforce can subscribe to. I have added the following External Objects to my data model [point to the Payment__x, Invoice__x, Invoice__Header__x, and Invoice__Line_Item__x objects on your data model diagram].

Invoice PDFs will be generated via a Batch MuleSoft job that runs every month, retrieves the invoice details from the ERPs, and passes them to the PDF Generator system to generate PDFs [point to Int-007 on your landscape diagram and your list of interfaces]. MuleSoft would then retrieve all files stored on the SFTP and transfer them to Salesforce [point to Int-008 on your landscape diagram and your list of interfaces]. We can use a specific file naming convention that includes the account ID. This will allow MuleSoft to attach the files to the correct Account record. The PDFs will be stored using Salesforce Files [point to the Content Document object on your data model diagram].

Next, we have a requirement to create a critical incident in case of energy failure and assign it to the right maintenance partner based on the property's address. The critical incident will be another Case record type. I will use Case Assignment Rules to assign the critical incident case to the right partner user or queue, assuming we have a field that indicates the relevant case region.

Once that is done, we need to identify all customers relevant to that particular region and send them all an SMS notification. This is a mass SMS send, a capability not available out of the box on the Salesforce Platform. Therefore, I will use Marketing Cloud to fulfill this requirement.

*We will send transaction emails and SMS using the Marketing Cloud Transactional Messaging API.*

*Once a critical incident case is created, we will fire a Platform event from Salesforce to MuleSoft with the necessary case details [point to Int-009 on your landscape diagram and your list of interfaces]. MuleSoft will then query Salesforce to find out the relevant/impacted customers, then invoke Marketing Cloud’s Transactional Messaging API [point to Int-010 on your landscape diagram and your list of interfaces]. MuleSoft can orchestrate the entire process in a scalable and performant way.*

*All communications between Salesforce and MuleSoft will be protected using two-way TLS and authenticated using the OAuth 2.0 web server flow (initially, then the Refresh Token flow going forward).*

This is a lengthy business process, but presenting its solution should take five to six minutes. The other business processes should take less time. Practice the presentation using your own words and keep practicing until you are comfortable with delivering a similar pitch in no more than six minutes.

Some questions have also been seeded, such as the full description of the OAuth 2.0 web server flow. If the judges feel a need to get more details, they will come back to this point during the Q&A.

### Note

You have included the assumptions in the context of the presentation. You can use this approach or simply list all your assumptions upfront. The former approach is preferable as it is more natural and engaging.

You are not going to create a pitch for the LDV mitigation strategy. However, you can utilize the table you created earlier in this chapter (*Figure 15.4*). It contains a list of all identified LDV objects, the expected number of records, and the mitigation strategy.

Move on to the next element of the structured presentation.

## Presenting the Project Development and Release Strategy

At this stage, you have probably consumed most of the time allocated for your *structured presentation*. Proceed with your concise project development and release strategy pitch:

*The company would like to start realizing value quickly. To achieve that, I propose following an MVP’s concept in defining the functionalities included in each release. The MVP allows enterprises to start gaining benefits without waiting until the end of the program. This effectively reduces the overall risk as the users will start using the new system quickly and get additional capabilities with incremental releases.*
