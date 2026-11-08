
- You accept the *risk* associated with a *purely emergent architecture* (unless also putting in place suitable governance mechanisms).
- You value continuously inspecting the solution and being able to adapt to changing requirements, new knowledge, or new uncovered key requirements *over* knowing the final cost and timeline.
- You believe you will want and need to do a lot of prototyping PoCs to understand what a solution will entail – *and accept this approach will require time and money*.
- You are aware of and accept the risks associated with a time and material contract model should you engage with an implementation partner to work with you in a pure agile delivery methodology.

You should consider choosing a form of a *hybrid agile* delivery methodology – instead of pure agile – if the following apply:

- Your organization requires *at least some indication* of the cost of your project.
- You see the value in *some level of architecture* being determined up front.
- You need to approach *some* elements of the project scope in a waterfall manner, such as legal requirements, compliance requirements, and integrations while needing to approach *other* elements in an agile way.

*In our scenario with PME, the Salesforce Taskforce’s project is charged with the goal of developing an MVP solution including capabilities for marketing, sales, and service, and rolling the solution out globally. The solution must have sufficient functionality that it can replace and retire a legacy CRM currently serving sales and customer service departments, and a simple email marketing system.*

## Project phasing approaches

In the olden days, deploying IT solutions was done in a big bang manner, deploying all functionality and having all users going live at once, celebrating in style with champagne and cake – or so I am told. For at least the past decade, it’s become the norm and mantra to build an MVP to release a working solution as soon as possible.

For many types of IT projects, it makes sense to build an MVP and release it as soon as the developed product can deliver value through a better feature set, experience, business outcome, and/or lower cost compared to the existing system being replaced. However, for enterprise IT projects – Salesforce implementation projects fall into this category – there are valid reasons to counter the urge to go live in production (the first go-live) prematurely.

The strongest argument to not rush the initial roll-out is that the moment you have users live in production, you will multiply your Salesforce setup and complexity by at least two-fold compared to being in the development phase.

Here are some of the key drivers and responsibilities you will have once you go live that you don’t – at least not to the same degree – in the development phase:

- User provisioning and deprovisioning:
  - Employees change roles and need new roles, permission sets, groups, and queue memberships
- The training of new users
- Managing the production environment
- Potentially managing a production support environment
- Regression testing key processes three times a year with every Salesforce release
- The handling of new feature requests from users

### The handling of bug reports from users

This can seem like a lot of work, and it is if it's not managed in the leanest way possible, process-wise, and architecturally. We’ll cover this further in *Chapter 12, Evolving Your Salesforce Org and DevOps Capabilities.*

I am by no means suggesting you push your initial release for as long as possible. I am simply stating *there is a consideration to be made for the right timing* – factoring in the time it takes to get these processes in place, some of which require technical setup to be done up front.

There are ways of phasing your implementation so as to not create a big bang where you are overwhelmed with the previously mentioned production responsibilities.

Two types of project phasing exist that at first may seem identical, but there are distinct differences.

### Phasing by scope

Different business domain capabilities are developed and released to relevant **functional groups of users**. For example, you could opt to develop and release the business capabilities for your *sales* organization and later develop and release for your *marketing* and *customer service* organizations. There are pros and cons to this approach, as highlighted here.

These are the advantages:

- Smaller scope means faster development and roll-out of value-adding, working solutions
- Acquired Salesforce licenses are utilized sooner than if the solution had to include the full scope, and wait for all capabilities to be developed before any single functional user group could start using Salesforce
- Less concurrent effort for training and change management
- Less compounded risk of any one business capability not working and delaying the deployment of the entire solution

These are the disadvantages:

- Risk of getting stuck optimizing a few capabilities beyond the return on investment at the cost of not developing new capabilities for other user groups.
- Potentially require temporary integrations to legacy systems to be developed to have a full customer overview. For example, if a Salesforce solution is initially built for sales users and doesn’t include customer service, the sales users won’t have access to their customers’ service history in Salesforce, and will either have to use two CRM systems (poor user experience), or a temporary integration needs to be developed to display service history from the legacy CRM in Salesforce.

### Tip!

As we progress through the activities in this chapter, bear in mind the maturity and the strategic importance of your different business capabilities, as they will guide you to prioritize the scope for your initial Salesforce release.

Let’s look at another strategy for phasing your Salesforce project.

### Phasing by geography

In this type of project phasing, users in one geography will be using all the capabilities developed in the project – but the roll-out (and migration) is phased.

For example, you may decide to develop an initial MVP solution, roll it out to the Australian and New Zealand organization, and then roll it out further to other regions or countries – in bulk or groups of countries.

These are the advantages of phasing by geography:

- Less concurrent effort for training and change management
- Less risk the developed solution does not sit well with any one particular country or region – impeding adoption

These are the disadvantage of phasing by geography:

- Risk of optimizing the solution for one country or region and not carrying out intended process harmonization

### Tip!

As a general rule of thumb, when a capability is developed, tested, and ready for use, prioritize rolling it out for feedback as soon as possible. Improve the solution based on the feedback received, and then roll it out further to as many geographies as possible in order to ensure the solutions built are adopted by your entire organization – so you don’t risk different geographies using different systems concurrently.

Having covered Salesforce project delivery methodologies, it’s time we talk about change.

## Determining your change management strategy

Over the course of your Salesforce project, many things will change for your organization. Most importantly, how your employees work will change. How good you are as an organization at managing these changes has a direct and enormous impact on the success of your Salesforce project. You could design and deliver the most technically sophisticated solution, but if your users don’t want, need, or know how to use your solution, then your efforts and investments will not bear fruit.

### Introducing change management

**Change management** encompasses all the efforts, initiatives, activities, and tools you use to maximize the positive outcomes of the changes your organization undergoes.

You need to manage change in your Salesforce project to ensure the intended users of your Salesforce solution adopt and use your Salesforce solution. The level of **adoption** is a key metric for measuring the success of your Salesforce project, which we’ll cover further in *Chapter 4, Securing Funding and Engaging with Salesforce and Implementation Partners.*

Adoption, however, is the *combined result of the collective efforts* you put into managing change. Let’s look at the reach of the changes that may come with your Salesforce project.

### Degrees of change in Salesforce projects

Enhancing business capabilities – through implementing Salesforce – *reaches beyond changing the system component* of a business capability.

Implementing Salesforce may change your business capability components in the following ways:

- **Systems**: Potentially replacing a legacy system with Salesforce will impact your overall system landscape, and likely *how your enterprise systems integrate*.
- **Data**: How data is managed, stored, distributed, consumed, and utilized is affected as part of a Salesforce project. In today’s **data-driven enterprises**, data is considered to be one of the greatest assets. You need to consider and map the stakeholders impacted by replacing the old way of working (legacy systems) with Salesforce and design your future data management and governance, and understand the
