
internal project leadership and sponsor to plan beyond the current implementation project, act as an escalation point for risks and issues, and be responsible for managing the commercial relationship and contract.

Next, let’s discuss how you can plan development.

## Planning your build phase

In a pure agile delivery methodology, the team plans their work continuously – either in sprint planning (Scrum) or by simply starting new work when new user stories meet the **definition of ready** and there is the capacity for it to be picked up (Kanban).

However, having chosen a form of hybrid agile delivery methodology, you’ll likely want to have some indication of the estimated duration of the build phase – both for budgeting and roll-out planning purposes.

In this section, we’ll discuss the advantages and disadvantages of various methods of **estimating development effort** and how you can plan development.

Let’s look at two methods of estimating effort:

- **Estimating development effort in time format:**
  - In the previous chapter, you broke down the overall scope into user stories and provided solutions for each of them. If you didn’t, go back and review this process.
  - To know the total number of estimated development hours, you would need to have each user story estimated (time: hours) by *development type* (configuration, data, and customization). Inherently, for an estimate to be somewhat accurate and hence useful for planning purposes, each user story needs to have a solution design done beforehand.
  - Once all stories are estimated, you summarize the estimates by epic and then summarize all epics by theme/domain to know the total.
  - It is important to note that, at this point, the estimate is simply an estimate. It is not until the story or sprint is finished that you will know exactly how long it took.

### Important note

When summarizing the estimate effort - and thus cost - in the development phase, it sometimes exceeds the original budget for your Salesforce project. This is typically not because the implementation partner is trying to squeeze you. It is usually because the consultants ask questions and uncover requirements, rules, exceptions, edge cases, and enablers, which weren’t apparent to you before the scope was detailed.

### Story points:

- Story points are radically different from estimating in a time format. A user story’s story points *describe a mix of total effort, complexity, risks, and uncertainties* expected for the agile team to refine, design, develop, test, and deploy and release a user story. As you can see, the story point concept extends far beyond the hour estimate model, which focuses on the time for the development of the user story.
- A key aspect of story points is that they are *relative to an agile team’s history of working together in the team*. This also means that assigning story points to all user stories upfront isn’t a meaningful exercise.
- Story points are typically assigned to user stories in sprint planning. We’ll return to this later in the *Maximizing value from agile ceremonies* section.
- Using story points is suitable for projects where less effort has been made up front to do user-story-level solution design.

After estimating your development effort, let’s move on to creating your development plan.

## Drafting your tentative development plan

Assuming you have a budget and a deadline for your project, you need a plan for your development to know how you will respect them.

Once you have each user story estimated, and thus each epic, you need to consider your available development capacity. This is the amount of time your functional consultants and developers have available for development tasks. You need to be mindful of the development estimates by development type. Next, you need to factor in time for agile ceremonies and possible backlog refinement, for which – although they are important sessions – the time is not available for development. The difference is referred to as sprint efficiency and is typically between 50% and 70% depending on your chosen delivery methodology and hence, the degree of upfront user story definition, detailing and providing solutions.

### Tip

As a general rule of thumb, you shouldn’t fill up more than ~85% of your agile development team’s net development sprint capacity (after applying sprint efficiency). The reason being when you progress through sprints – and see your solution come to life – you will be inspired to do more things or to modify a process that made sense in workshops but isn’t ideal for users after all.

If the development capacity (resources) available in your project doesn’t match your total estimated effort or timeline, you have three options:

- *Prioritize* what user stories are in the scope of the initial release of your Salesforce solution.
- Work with your implementation partner to *find alternatives* – more manual solution options – for some user stories.
- *Increase the budget* by allocating additional development resources to deliver the project according to your initial timeline. There is a limit to how many development resources you can add and still gain added capacity. It will depend on the nature, scope (number of domains), and the sophistication of your development setup. We will cover development model and process in the next section. Alternatively, you can consider prolonging the build phase to be able to deliver the scope.

Having determined your scope and available development capacity, you are ready to create your tentative development plan by assigning epics to sprints accordingly by taking dependencies into account.

Let’s have a look at an example from a scenario company, PME:

![](img/307881206539c45e128c538a4b6912f8_185_0.png)

Figure 7.1 – PME’s tentative development plan

*PME’s development plan has dependencies between its functional epics and technical enablers. In addition to the functional scope, the technical foundation to start development needs to be established before actual development can commence. This includes setting up technical tooling for the development pipeline, creating development environments (sandboxes), and creating users and test users for the development team and the PO.*

After you’ve created your development plan, let’s get your development life cycle and deployment process set up.

## Setting up your development model and environments

The **development model** and environments are part of your **deployment model**. Deployment means moving the technical components of your solution from one environment to another and eventually to production. You use different environments – **sandboxes** – to perform different types of testing in your development life cycle. The purpose of this is to make sure your solution works end to end as intended when it reaches production.

Let’s begin by looking under the hood of Salesforce.

## Understanding the components of your Salesforce solution

The products Salesforce offers, such as Sales Cloud and Service Cloud, are cloud-based **software-as-a-service (SaaS)** products. That means your Salesforce org – depending on the licenses you have bought – comes with standard objects and standard fields **out of the box (OOTB)**.

Salesforce is also a **platform as a service (PaaS)**, which means you can extend, enhance, configure, and customize the OOTB components within Salesforce to fit your business needs and processes. This includes creating custom objects and fields and automation, configuring and customizing the **user interface (UI)**. You can also use the Salesforce platform to build custom apps and integrations. The configuration and customization capabilities that allow you to quickly configure and customize Salesforce to fit your business needs are one of the things that makes Salesforce a powerful platform.

To better understand the configuration and customization of your solution, we need to understand the underlying architecture of the core Salesforce platform, which is based on a **model-view-controller (MVC)** architecture.

Let’s have a deeper look at what MVC consists of by seeing some examples:

|  | Model (data structure) | View (UI) | Controller (logic) |
|---|---|---|---|
| OOTB components | Standard objects, standard fields, and standard picklist values | Standard apps, default page layouts, and Lightning record pages | Standard duplicate/matching rules and lead conversion |
| Configuration components (no-code) | Custom objects and custom fields (including relationship fields), custom metadata types, and custom settings | Custom apps, page layouts, Lightning record pages, and screen flows | Business logic: flows, process builder (retiring), and workflow rules (retiring) Integration: connectors, flow with outbound messages and enhanced External Services |
| Customization components (low-code/pro-code) |  | LWCs, Visualforce (VF) pages, Aura components, Canvas Apps, and iFrame | Business logic: Apex triggers, Apex classes, and Salesforce functions Integration: Platform events, |
