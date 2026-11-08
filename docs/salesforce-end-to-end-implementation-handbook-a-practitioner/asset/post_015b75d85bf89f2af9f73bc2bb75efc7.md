
### Tip

Unless your project is a greenfield implementation – meaning it’s the first time your company will use Salesforce – you should *limit the prepare phase* to a maximum of a few months for larger projects (for example, new clouds such as Field Service or major products such as CPQ) to ensure requirements input and target processes are still relevant.

For the sake of simplicity, this section of the book will focus on a *hybrid agile delivery methodology* in which some activities are done to detail the scope of the Salesforce project and create upfront architecture before starting the build phase.

### Important note

The milestones and activities after project kickoff will either happen in sprints (pure agile), stages (waterfall), or a mix (hybrid agile).

To refresh the differences, please see *Figure 3.2* in *Chapter 3*, *Determining How to Deliver Your Salesforce Project*.

Let’s see who will be overseeing your Salesforce project.

## The role of your Salesforce CoE in the development phase

Recall the *Salesforce taskforce* that you were encouraged to establish to drive the activities in the pre-development phase. It included resources from three key areas of your company: business, technical, and project management office. *You* likely reside in one of these functions.

Your Salesforce taskforce has established the capabilities and Salesforce clouds in the scope of your project – and secured a budget for at least the initial release. Your Salesforce taskforce has also contracted with an implementation partner to support delivering the initial release.

Your Salesforce taskforce shouldn’t be shut down after the pre-development phase, rather it should evolve into your Salesforce **Center of Excellence (CoE)**. The members of your Salesforce CoE are well suited for *governing the delivery of your initial release*.

In *Chapter 3*, *Determining How to Deliver Your Salesforce Project*, you envisioned the structure and responsibilities of your Salesforce CoE. Now it is time to execute and establish it.

The next activity – in the hybrid delivery methodology – is to break down the business capabilities into smaller backlog items that your implementation partner’s developers and functional consultants can consume and deliver working solutions for.

## Detailing the scope of your initial release

Your project’s next endeavor consists of translating the vision and capabilities – business processes – into product backlog items. Breaking down the business capabilities into smaller backlog items requires you to involve the end users of your Salesforce solution through a series of workshops.

In *Chapter 3*, *Determining How to Deliver Your Salesforce Project*, we discussed the three overall delivery methodologies to choose from – *pure waterfall*, *hybrid agile*, and *pure agile*.

As we are covering a hybrid agile delivery methodology in this section, let’s consider the activities to prepare for development:

- An effort is made to establish foundational architecture including – but not limited to – determining a Salesforce org strategy, integration, data migration, data backup and archival, and reporting strategy, as well as a development and environment strategy
- Detail the business processes (the customer and user journeys), in the scope of your project, and create a solution design for it

### Tip

In hybrid agile – after establishing the foundational architecture – you can opt to detail all processes in the scope of your project – along with solutions and estimates. Alternatively, you can decide to detail a sufficient number of backlog items to fill the first couple of sprints, and, concurrently with the sprint development team, dedicate resources to continuous backlog refinement in a parallel design stream.

Let’s get started detailing your company’s future business processes.

## Co-creating your future business processes

You may recall the activities in Chapter 2, Defining the Nature of Your Salesforce Project, where you facilitated workshops to determine current capabilities, their processes, and the current pain points and suggestions from the stakeholders.

Whereas the purpose of those workshops was to determine and understand current processes, it is the purpose of the workshops in the development phase to create future processes supported by Salesforce.

Plan and facilitate workshops for each of the capabilities – processes – in the scope of your initial release. Review the workshop structure proposed in Chapter 2, Defining the Nature of Your Salesforce Project, but follow this proposed agenda:

- **Welcome and introduction:** The facilitator welcomes the participants and introduces the agenda and goal, and the participants introduce themselves:
- **Workshop goal**: Share the vision statement you crafted in *Chapter 1, Creating a Vision for Your Salesforce Project*
  - You are seeking your participant’s valuable input to shape your company's future CRM system
- **Exercises**: Divide the overall workshop goal into smaller exercises divided by adequate breaks:
  - **First exercise**: Review the current process as you defined in *Chapter 2, Defining the Nature of Your Salesforce Project*, and review the pain points and suggestions for improvement:
    - This approach – while valuable in itself – has the added benefit of creating a sense of urgency and sparking the desire for change
  - **Second exercise**: It’s time to start with a clean slate and begin co-creating the to-be processes. This can be daunting – staring at the blank canvas as a group. Here are a few methods to help you get started:
    - Determine the start and finish of the overall business process by briefly discussing what has happened before the starting point of the process, and what will happen after the finishing point.
    - State the overarching phases in the process: for example, creation, assignment, execution, and post-execution.
    - Create **swim lanes** by determining what **personas** are part of the business process – you may reuse the personas you determined for your current process.

### Tip

Keep the goal and focus of the workshop in mind, and guide your participants so they don’t get stuck overanalyzing minute details of each process step.

- As mentioned earlier, pay *extreme attention to variations*. If your solution is to be used by multiple regions and/or lines of business, be sure to include stakeholders from each – where practically possible – if it is the ambition of the Salesforce project to harmonize business processes.

### Tip

Ensure you align with your implementation partner before designing the workshops to determine each party’s roles and responsibilities to maximize the output of the workshops. If you are working with an implementation partner, they should be able to provide experienced business analysts and/or architects to facilitate and drive the workshops.

Depending on the scope and nature of the business capabilities in the scope of your Salesforce project, an experienced **Salesforce business analyst** may suffice to support your business stakeholders in co-creating your future business processes supported by Salesforce. For greenfield implementations, and for more complex implementations, be sure to include a **Salesforce solution architect** in the workshops. Their input and experience will make sure the proposed processes make sense from a Salesforce perspective.

Let’s look at the next steps after each workshop.

## Crafting user stories

Following each workshop, your PO or your implementation partner will use the co-created future business processes to create backlog items, also known as **user stories** for each epic or business process.

A user story should be an easy-to-understand sentence describing – from a *user’s perspective* – *what* they want to be able to do and *why*. Let’s break it down:

- **Persona**: They may be internal or external persons who are part of the business process:
  - Internal persona examples: An employee in a specific function
  - External persona examples: A customer, partner, or supplier
- **Need**: What does the persona want to be able to do in the process step?
- **Purpose**: What business outcome does this achieve?
- Be mindful that the purpose should tie back to the overall business objectives of the project. The purpose of a user story *is not* merely to be able to proceed to the next step in the business process.

A user story should be written for each step in your business process.

Some considerations to keep in mind when writing user stories are as follows:

- They should not be written in Salesforce-specific language, rather it should be technology and system agnostic
- They should focus on the business need, problem, and desired business outcomes, and not describe a desired solution

Sticking to the preceding advice will ensure the expert consultants at your implementation partner seek to find the best solution for your business.

### Important note
