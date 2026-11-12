# Presenting the Data Migration Strategy

Stick with the four points listed earlier to explain the data migration strategy. You need to explain how you will get the migration data, how to load it into Salesforce, and what your migration plan and approach look like. Start to put that together:

PPA has around 50 million rental records in each of its retiring five rental management systems. That is a total of approximately 250 million records. Many of them are historical records that should ideally be migrated to Heroku rather than Salesforce.

I propose using a staggering big bang approach. We will migrate each retiring rental management system’s records using a big bang approach, then proceed to the next retiring system. The migration activities will occur on weekends, where the entire system will be shut down to all users except those performing the migration.

We will execute a migration dry run on the Full Copy sandbox three days before the migration to production to spot potential issues. Before migrating the data, we will disable all Workflows, Flows, and other automation.

We will use Informatica to do the data migration. It is optimized to use the Bulk API, which should speed up the migration process. We can connect Informatica directly with the source systems to retrieve data from the source database. But the type of the database is unclear. Therefore, I am assuming that PPA would extract the data as CSV files based on a template shared with them.

All newly migrated records will be owned by a user who does not have a role. This will mitigate the ownership skew. Later, we will assign the migrated records to their rightful owners based on a planned schedule. This process will take place gradually every night to avoid impacting the platform’s performance by calculating a huge number of sharing records.

After completing the migration for each region, we will run some tests to ensure that we have all the migrated records in the target systems. Again, the migration will be done using Informatica to both Salesforce and Heroku, where open rental records will be migrated to Salesforce and closed ones will be migrated to Heroku.

That covers the data migration strategy and plan. You must mention these details to collect this domain’s points. Again, remember to keep an eye on the timing. You should not consume more than 30-35 minutes of your valuable 45 minutes to cover the structured part of your presentation. This will leave you with 10-15 minutes for the catch-all part, which is very important to avoid losing any easy points. Proceed to that part and see how it works.