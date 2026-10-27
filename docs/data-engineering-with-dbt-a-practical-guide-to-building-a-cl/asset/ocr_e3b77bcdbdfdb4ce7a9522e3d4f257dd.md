dbt allows you to deploy any environment to a database and target schema of your choice, giving you the freedom to arrange your environments.

The main decision is if you want to have one database for each environment or if you want to host some or all in the same database and differentiate them based on the target schema.

In the case of Snowflake, the difference is minimal as you can easily query across databases and the same role can have privileges across multiple databases. However, in other cases, the difference can be much bigger.

If you decide to have the QA and PROD environments in two different DBs, we suggest using the same target schema for both environments so that the schema and table names will be the same in the two environments.

This will make it very simple to switch clients, such as BI applications, so that they use QA and PROD by just changing the connection string. As an example, this allows you to easily test a report in QA and then switch it to PROD by changing the database you point to.

In terms of what code to run when using the dbt Cloud IDE, this is straightforward as you either name a specific custom branch to be used for the environment or the environment will use the main branch.

As a note, consider that you can start jobs on any deployment environment using the dbt Cloud API. In that case, you can override most of the environment settings by providing the optional values in the REST call.

# Creating jobs

Now that we have created the environments, it is time to see how we can effectively deploy our code. This is done by creating dbt jobs that run dbt commands.

In essence, a dbt job is a set of commands that you want to run on an environment.

You use jobs to organize the operation of your data platform by selecting the commands that you run and deciding on what part of your project each command is applied to by using the dbt node selection syntax (see https://docs.getdbt.com/reference/node-selection/syntax for all the details).

Jobs can be scheduled, and they will be triggered by the internal dbt Cloud scheduler, or they can be triggered through an API call. You can also have a job triggered by a webhook when a Pull Request (PR) is created or updated if you are using one of the Git provider dbt integrations (currently GitHub, GitLab, or Azure DevOps).