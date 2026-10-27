# Deployment environments

We use deployment environments to create the common environments that are maintained through automation, using jobs.

In very simple realities, you can define only one deployment environment pointing to the `main` branch’s code. If you do not specify a custom branch, main will be used.

This is very simple but is only suitable when you have only one or two developers, making a few changes with no overlapping work and an eventual downtime in the data platform is acceptable as you will need to rely only on the testing done in the individual developer’s development environment, without any integration testing.

In this simple setup, once you merge a PR, the code is immediately deployed and your users will see the changes after the next project run.

In general, you will want to create more environments, with our common setup being the one presented in the previous chapter – that is, with one QA environment connected to the `main` branch and a production environment connected to a PROD branch.

In more structured setups using many long-lived branches, such as a full Git Flow with release branches, it will be natural to create more environments.

In dbt, you can create as many environments as you wish – there is no limit. However, in general, an environment is created by association with a long-lived branch that has a specific semantic in the organization, such as the PROD branch, which represents the production environment in our sample project.

You can have multiple environments associated with the same code branch. It can serve special cases where you want to freeze a branch at some point in time without blocking another environment defined on the same branch or you want to deploy the same code in different places. As an example, this can be used to serve audits or stress tests as you do not want your reports/results to change halfway through the process, but you do not want to stop development either.

# Creating our QA and PROD environments

In this section, we will use our sample project to create two deployment environments. The QA environment will be associated with the `main` branch, while our production environment will be named PROD and it will be associated with a PROD branch that we will create with GitHub.