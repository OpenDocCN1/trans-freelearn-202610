
Starting with the out-of-the-box functionalities, it is easy to recognize that you can utilize the standard lead process functionality to define multiple lead processes, each with different lead statuses. At each stage, you can generate a set of tasks and assign them to the lead owner. This can be achieved using one of the following (in order):

- **Flows**: Salesforce Flows can be used to execute complex logic upon creating or updating a record. This is a configurable and scalable tool that can deliver the desired functionality. Moreover, it is a modern tool that Salesforce is actively enhancing with each release. Clearly, it has a better chance of receiving more future enhancements than any other tool on this list.

- **Custom Apex code**: This gives the ultimate capability and flexibility but with unjustified additional development and maintenance costs.

**Note**
Salesforce has retired Process Builder and workflow rules as of the Winter '23 release. While the current workflow rules and process builders will continue to run, you will not be able to create new automations using these tools. These tools should not be considered part of your solution.

Your proposed solution could be as follows:

I propose utilizing the lead process functionality to define multiple lead processes with different lead statuses. Then, create a Salesforce record-triggered flow to automatically create tasks and assign them to the lead owner upon updating the lead to a specific status. The flow can be configured to utilize criteria-based branching logic, and this will allow us to create a different set of tasks depending on the lead status.

Once the lead is qualified, it must be converted into an opportunity and automatically handed over to the country's sales operations team.

This requirement does not specify whether the lead conversion should happen automatically or whether it is fine to stick with the manual process. You can assume one of these and base your solution on it.

By default, when the opportunity is created out of a lead conversion, it is owned by the same user who owns the converted lead. You need an automation process to assign it to the sales agent's queue in that country.

## Your proposed solution could be as follows:

*The scenario did not specify whether the conversion has to happen automatically; therefore, I am assuming it is done manually by the lead owner. Once the lead is converted into an opportunity. I will utilize a Salesforce record-triggered flow to set a sales agent from the target country as the owner of the opportunity. I am assuming that there is a pre-defined user specified for each country. The flow will also add the country’s sales operations users to the opportunity team. I am also assuming a limited set of users per country.*

*The sales operations agent will follow up with the deal. The agent should be able to create quotes and negotiate them with the client.*

This is standard Salesforce functionality. The sales agent has a Sales Cloud license, therefore, can work with the quote object.

## Your proposed solution could be as simple as the following:

*The sales agents will be assigned a Sales Cloud license; this allows them to create and update quotes. This is standard Salesforce functionality.*

Remember to use your diagrams while explaining the solution. The actors and licenses diagram (such as the one you saw in Figure 8.1) is particularly useful in this case.

*Once a quote is accepted, the agent updates the opportunity to the contracting stage.*

The main requirement is to automatically create a contract out of the closed opportunity, which can easily be done using flows or custom-developed code. The scenario did not specify any advanced contract creation logic to justify proposing a third-party solution.

There are several **CLM** solutions out there on the market, including some available on AppExchange, such as Apttus Contract Management, DocuSign CLM, and Conga Contracts. They provide advanced contract creation and negotiation functionalities. However, that has not been requested in this scenario.

> **Note**
>
> You need to memorize the names of some common third-party products from the Salesforce ecosystem, such as CPQ tools, integration middleware, identity providers and identity management solutions, e-signature solutions, document generation solutions, advanced lead assignment solutions, CTI, version control, release management, backup and restore, and so on.
>
> You will encounter examples throughout this book whenever applicable. As a CTA, you should make yourself familiar and keep up to date with what is available on AppExchange.

Your proposed solution could be as follows:

There has been no requirement shared about advanced contract creation functionalities. Therefore, I am assuming a simplified solution that utilizes a Salesforce record-triggered flow to automatically create a contract once the opportunity is updated to the mentioned stage.

The agent should ensure the contract’s data is accurate, select the right terms and conditions, and send the contract to the client for an online signature.

Here, you come across a requirement for an e-signature, a functionality that would require extensive efforts to develop. This is totally unjustified, considering that there are several battle-tested solutions available on the market and via AppExchange, such as DocuSign, HelloSign, and Adobe Sign. These products can also help to fulfill the second part of the requirement, which indicates that the contract should be presented in the CRO’s local language.

Your proposed solution could be as follows:

To fulfill this requirement, I propose using a third-party application from AppExchange, such as DocuSign. This will allow selecting the right contract template with the appropriate language before sending the contract over to the customer. The tool will also allow the selection of suitable terms and conditions to include in the contract.

Update your landscape architecture diagram and move on to the next requirement.

The client should receive a request to sign the document online.

Once the document is signed, the contract should be made active, and the opportunity should be updated to closed-won. Luckily, most of this requirement is automatically fulfilled using the third-party e-signature tool you suggested. You still need to explain how that works, though.

Your proposed solution could be as follows:

This capability is built into the proposed e-signature solution. Using DocuSign, you can update the contract status once the document is signed. Then, you can use a Salesforce record-triggered flow to update the opportunity stage back to closed-won. The flow will kickstart once the contract status is updated.

Now, move on to the next requirement, as follows.

The agent should update lost opportunities to the closed-lost stage manually and set a reason for the loss.

You can assume that the loss reason is captured using a custom field. You then need to introduce a mechanism that ensures this field becomes mandatory when the opportunity is updated to the given stage. You can utilize field dependencies and validation rules to ensure that.

## Your proposed solution could be as follows:

To fulfill this requirement, I propose introducing a custom picklist field with predefined values for the possible reasons for losing an opportunity. Then, utilize the standard field dependency feature to make this field dependent on the opportunity stage value. I also propose introducing a custom validation rule to ensure a value is always entered in this field when the opportunity is updated to the closed-lost stage, regardless of whether the update took place via the UI, code, or an API call.

Once the opportunity is closed-won, a project that contains key information derived from the opportunity should be created automatically.

Do not expect the requirements to be isolated based on their related domain. In many cases, you will come across requirements that cut across different domain knowledge, like in real life. In this, you have two requirements—one related to solution architecture and another to security.

## Your proposed solution could be as follows:

To fulfill this requirement, I propose to use a custom object to store the project details and use a Salesforce record-triggered flow to create and populate a record of this object once the opportunity is closed. I considered using one of the standard objects, such as the case object, to represent a project but decided to use a custom object as there were no shared requirements that justify using the case object, such as SLA requirements or escalations. Moreover, I assumed that the case object might be utilized for customer support-related topics in the future and preferred not to repurpose it.

The custom object's OWD will be set to private to restrict the record visibility to its owner, which by default will be the sales agent. Considering that the record will be created upon closing the opportunity, which is an action carried out by the sales agent, I assumed that it is acceptable to keep this record visible to the sales agent.

I propose introducing a role hierarchy to include the nurses and their managers. Then, use criteria-based sharing rules to share the project record with the nurses, granting their managers automatic access.

> Note
>
> This scenario does not have many specifications for data sharing and visibility and does not provide enough details to design a role hierarchy. In this case, you could make some assumptions, such as assuming a role on top of the nurse managers that report to the CEO.
