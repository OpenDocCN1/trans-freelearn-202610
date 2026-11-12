
Your actors and licenses diagram could look like the following:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_522_0.png)

Figure 15.10 – Actors and licenses (final)

Your data model diagram could look like the following:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_523_0.png)

Figure 15.11 – Data model (final)

No changes occurred in the role hierarchy diagram.

Congratulations! You managed to complete your second full scenario. You have created a scalable and secure end-to-end solution. You need to pair that now with a strong, engaging, and attractive presentation to nail down as many marks as possible and leave the Q&A stage to close a few gaps only.

Ready for the next stage? Proceed forward.

## Presenting and Justifying Your Solution

It is now time to create your presentation. This is something you would have to do on the go during the review board as you would likely consume the whole three hours creating the solution and supporting artifacts.

As you learned before in Chapter 13, Present and Defend: First Mock, you need to plan your presentation upfront and use your time management skills to ensure you describe the solution with enough detail in the given time. You will learn some time-keeping techniques in Appendix, Tips and Tricks, and the Way Forward.

You are now familiar with the structure of the board presentation. You start with the structured part of the presentation and then switch to the catch-all mode. As a reminder, these two different presentation phases include the following:

- Structured part: In this part, you describe specific pre-planned elements of your solution, starting with an overall solution introduction, including a brief walk-through of the created artifacts. Then, you start tackling the business processes one by one—and using the diagrams and artifacts—explain how your solution will fulfill them. After that, you need to describe your LDV mitigation and data migration strategies.

You can add more topics as you see appropriate, assuming you have planned and budgeted enough time for that.

- Catch-all part: This is where you go through the requirements one by one and call out those that you have not covered in the first part. You do this to ensure you have not missed any requirements, particularly the straightforward ones.

In this chapter, you will create presentation pitches for the following topics: the overall solution and artifact introduction, one of the business processes, and the project development requirements. The customer service process will be used as it is the most complex and lengthy.

You will then hold your ground and defend and justify your proposed solution, knowing that you might need to adjust the solution based on newly introduced challenges.

In the following sections, you will be creating sample presentation pitches for the structured part. Start with an overall introduction of the solution and artifacts.

### Introducing the Overall Solution and Artifacts

Similar to what you did in Chapter 13, Present and Defend: First Mock, start by introducing yourself; this should be no more than a single paragraph. It should be as simple as the following:

My name is [your name], and I am here today to present the proposed solution for [the company's name in the scenario].

Then, proceed with another short paragraph that briefly describes the company’s business. This will help you to get into presentation mode and break the ice. You can simply utilize a shortened version of the first paragraph in the scenario, such as the following:

The scenario describes a European electric utility company that serves several cities across Germany, Italy, France, Portugal, Belgium, and the UK. The company operates in 40 cities. It offers services to both B2C customers (residential) and small and medium B2B customers.

Remember that you should use no more than a minute to present yourself and set the scene. Now you can start explaining the key need of the company using a brief paragraph, such as the following:

The company has been struggling with its existing CRM solutions for many years, and, as a new strategic movement, decided to switch to Salesforce. It is looking to use its new CRM to launch a global, unified new set of sales and service processes.

It is now time to explain the current and to-be landscape architecture. Use and show your landscape architecture diagram to guide the audience through it; never just stand next to it and speak in a monotone. You should be able to look at your diagram and describe the as-is and to-be landscape. Your pitch could be as follows:

The company uses six different ERP systems. They are used for service fulfillment, customer onboarding, switching, and activation. The plan is to retain all of them and integrate the new solution with them using their APIs. It is worth mentioning that the Belgium ERP does not offer an API.

Data will be traveling from Salesforce to these systems and vice versa. To develop a scalable and flexible architecture, we should aim to have a unified communication interface between Salesforce and the ERP systems. Middleware such as MuleSoft would provide that capability. It will also offer the ability to orchestrate the data flow between these systems and handle any encountered exceptions.

MuleSoft will also be able to communicate directly with the flat-file database that the Belgium ERP is based on using its built-in connectors. MuleSoft will not be transferring a massive amount of data. This is what pointed me to the need for an ESB rather than an ETL. However, I am also proposing an ETL to handle data migration.

The company uses a heavily customized CPQ solution that they are planning to retain. We will integrate with that solution using fine-grained APIs exposed by MuleSoft as the solution itself offers a poor API. Power Sales will be invoked at the right stage to calculate the price for a custom quote.

The company also relies on electricity and gas smart meters and regular radar-based meters that need to be supported. MuleSoft will again play a key role here as a unified integration middleware.

The proposed solution is replacing a set of legacy CRM solutions across the target countries. We will explain the data migration strategy later, but we propose an ETL tool such as Informatica to facilitate that.

The proposed solution is based on a single Salesforce org. It is understood that the company has recently acquired a new company that is also using Salesforce as its core CRM solution. But I believe that the two businesses are different and would likely need different sets of business processes. Furthermore, I assumed that the acquired company would continue to operate independently for a few years.

Based on that, I believe that we can start with a single org to accommodate the company’s requirements. This org will host all the functionalities described in this scenario. The acquired Salesforce org will continue to operate independently, and MuleSoft will be used to link that org with other systems.

The proposed solution will also utilize Marketing Cloud for customer segmentation, email, and mobile communications. We will also use some third-party applications from AppExchange, such as DocuSign.

I have created a full list of required integration interfaces and the authentication mechanism for each. I have also created a data model diagram, which I will explain further during this presentation.

You have set the scene and briefly explained your solution. Remember to call out your org strategy (and the rationale behind it) during your presentation. You can now proceed by explaining the licenses they need to invest in. Switch to the actors and licenses diagram and continue with your pitch. Remember to use your body language to assert your authority on the stage by moving across it (if you are doing the presentation in person). Your presentation could be as follows:

I have identified the following actors. They will need different types of licenses based on their expected roles. For example, the field sales agents [point to them on the diagram] will need a Sales Cloud license because they need to manage the new and renewal opportunities for customers.
