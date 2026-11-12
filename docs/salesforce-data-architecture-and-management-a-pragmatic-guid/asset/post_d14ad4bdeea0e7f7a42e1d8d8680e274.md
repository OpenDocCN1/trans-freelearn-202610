
- Data integration: This involves combining data from different sources to provide a unified view to the user. For example, a business is using Salesforce as its Customer Relationship Management (CRM) tool but Systems, Applications, and Products (SAP) as the order fulfillment tool. In the absence of any self-service tools where the customer can check the status themselves, sales representatives would need to know the order status so they can keep the customer updated. In this scenario, as the order goes through the different stages in SAP and the status changes, the status can be reflected in Salesforce.

With the definitions out of the way, let's look into the need for assessing data migration.

## Components of a data migration assessment

Data migration should not merely be a single line item in the project plan because it is nuanced and requires careful planning and execution. Having it as a minor line item in the project plan is a sure way to run into issues when running the migration into the production environment. Rather, it should be treated as a major component of any project with its own detailed schedule, tasks, and dependencies.

With a proper assessment, you can gauge the landscape and provide realistic time and budgetary estimates that can ensure a quality data migration. Let's discuss some of the integral components of a sound data migration assessment:

- Volume of data: This is usually the first thing that most project teams think about during data migration discussions. It is an important consideration since migrating 50,000 records will require a different approach than migrating 100 million records. For example, the choice of APIs, whether to use the Bulk or the REST API, will make more sense for the latter use case (migrating 100 million records) rather than the former.

- Type of data: Another consideration is the type and format of data that you are planning to migrate. For example, migrating leads from the legacy system will be much less work than migrating accounts, contacts, opportunities, and products. This is because of the potential relationships between these objects in the legacy system and the different data types and formats of the fields. More effort will be required if the Stage values, for example, are different in the legacy system than in Salesforce.

- **Number of systems**: What is the number of systems involved in the migration – are we migrating data from one source system or multiple source systems into Salesforce? For example, in a consolidation type data migration, you could have Oracle NetSuite and/or Microsoft Dynamics as the source systems, each with its own data types and standards. Migrating this into Salesforce will be much more involved than, say, simply migrating data from Oracle NetSuite. This may also necessitate that a staging database is introduced in the landscape where de-duplication and transformation can take place before loading the data into Salesforce.

- **Privacy**: With new privacy regulations getting introduced around the world and existing ones getting revised, what are the considerations for data migration during the preparation phase. Can offshore teams have access to it? Where should the infrastructure used during data migration be located – can it be in a country that may not have very robust privacy laws or doesn’t enforce them effectively? These are all considerations that could have an impact on the duration as well as the cost of the data migration project. Understanding this and any restrictions in accessing the data, whether they’re based on the client requirements or for compliance reasons, will help you plan for and address these requirements properly.

- **Data retention**: Not all data from the legacy system may need to be brought over into Salesforce but the business may have requirements around data retention for compliance purposes. Working with architects and other stakeholders, the project team would need to provide solutions to identify the optimal ways to stay in compliance and provide continued data access to the business.

- **Resource availability**: Data migration is never an exercise in isolation where the data migration team goes away, works on the project, and returns with a finished product that when executed in production will flawlessly migrate data. It must involve business resources who are intimately familiar with the data and can guide the team and validate the data migration as well as the production run. Understanding upfront what the business resource constraints are can help in identifying potential risks and provide the opportunity to put in place an effective mitigation strategy.

- **Current landscape**: This involves assessing whether all systems under consideration are in the cloud or on-premises. Are there any special considerations with respect to access to these systems, and where are the regular planned outage schedules? On-premises systems usually require more configuration because of the need to reach into the internal network. This would also include coordination with any third-party vendor resources whose assistance may be required during the migration.

- **Communication**: A good migration project will always have a solid communication plan that considers the needs of different stakeholders and how they like to be communicated. Depending on the scope, complexity, and the number of stakeholders in the data migration project, the communication plan can be very extensive. That shouldn’t deter you from creating a well-thought-out communication plan as it provides a roadmap of how other stakeholders will be kept up to date and what is expected of them during the project.

- **Document data transformations**: Once you have identified your source data and mapped it field by field to the destination fields, ensure that this is sufficiently documented. Ensure that you have mapped both the tables from the source to objects in Salesforce and the fields from the source table to fields in Salesforce. This document becomes a critical document for any issues down the road that may be due to incorrect field mappings. It also forms the basis for any transformations that are required and can be used by the testing team to validate your test loads.

- **Recurring exercise**: It should be made clear right from the get-go that data migration is a recurring exercise. It means that during the execution phase, multiple iterations will need to be conducted to get to a stage where the team feels comfortable with pulling the trigger in the production environment.

- **Data quality**: Data quality issues are a common problem in today’s systems and hardly any system is immune from it. When migrating data into Salesforce, consider the data quality issue of the source systems. This will help you understand how much effort will be required to cleanse the data before it can be loaded into Salesforce. Since data is used for decision-making and, among other things, automating business processes, the data quality of the source systems is of utmost importance during this assessment phase.

The preceding discussion is not intended to be exhaustive but lists some of the more critical aspects of the assessment phase. Each data migration project is unique and properly assessing the requirements and understanding the variables will increase your chances of successful data migration.

Let's now review the preparation phase of data migration.

## The Preparation phase

In this phase, you are prepping everything that’s needed for the next phase, which is the execution phase. Armed with the trove of information that was gathered in the previous phase, you are set to act on it and prep for successful data migration.

Let's discuss the best practices that apply to this phase.

## Best practices

You have developed a sound understanding of the current landscape along with understanding the type of data that is in scope, the source systems and their constraints, and the business requirements of the data migration. Now it's time to put all of this into action by following the best practices described as follows:

- **Analysis**: Carefully analyze the result of the exercise from the assessment phase. Seemingly minor details that are skipped during analysis can cause a lot of rework sometimes down the road. For example, assuming that a legacy system has good data quality; without vetting this assumption with the users or sampling the data to validate it can cause the data cleansing exercise to be a prolonged and expensive effort.

- **Set up data migration environments and tools**: Based on the requirements gathered so far and the analysis performed, set up the environment and the tools that are to be used to migrate the data. This will include any staging databases, data cleansing tools, de-duplication tools, sandboxes, and so on.

- **Test plan**: This is a critical aspect of the preparation phase because you want to have a clear idea of what is going to be tested (the scope of the test and the use cases to test), how it will be tested, and where (tools for testing and applicable sandbox environments), who will do the testing, and what the dependencies for testing are.

- **External IDs**: These are your best friends, so make use of them for easier data migrations. External IDs provide not only a method to keep track of records from the source system but also the ability to leverage the Upsert functionality in Salesforce.

- **Respect the loading sequence of objects**: This pertains to ensuring that your parent object is loaded first before the child object can be loaded. This means that if you are using spreadsheets to transform data as a staging area, you need to load accounts before contacts or opportunity records can be loaded.

- **Hardcoding IDs**: Record IDs in Salesforce change by environment and therefore, you should avoid using any hardcoded values in your loads. For example, a record type ID for an opportunity in a sandbox will be different from the ID in the production instance. If you must store IDs, have a configuration type file that is referenced during the runs. This gives you the ability to update the ID once in a centralized place that is then referenceable for all runs. Otherwise, you would need to update the record IDs for individual rows as you move through each environment.
