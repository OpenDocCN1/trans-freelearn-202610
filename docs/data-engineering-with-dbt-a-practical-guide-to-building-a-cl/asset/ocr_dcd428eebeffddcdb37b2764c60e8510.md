# Configuring our project and where we store data

The schema that you define in your dev credentials is the schema where your development environment will exist and, if we do nothing, everything we create will be generated in that schema.

Note that the schema for our DEV environment or a deployment environment is often referred to in the dbt documentation as target schema and it is available in the code as {{ target.schema }}.

Over time, having everything generated in the same single schema will become chaotic, and because one of our main goals is to create and maintain an environment that makes life easier for the developers, let’s fix it and start creating a bit more order by configuring different parts of the dbt project to be created in different schemata in our environment.

The problem is that our environment is defined as a schema and, in a database, you cannot have a schema nested into another schema, so we cannot create multiple schemata inside the target schema for our DEV environment.

To overcome this problem, dbt will generate the tables and views in multiple schemata that have the name composed by the name of our target schema followed by the name of the custom schema that we have defined for the specific content, with the final name of the schema having the following layout: <target_schema>_<custom_schema>.

As an example, if my target schema is RZ and the content is configured to go into a custom schema named REFINED, then the actual schema name when the content is deployed in my environment will be RZ_REFINED. The same content deployed to an environment that has a target schema of PROD will be created in the PROD_REFINED schema.

To recap, remember that you define the target schema where you define the environment, usually along with the connection details, while you configure the custom schema in the main dbt config file, or you override it along the project, as with any other configuration.

# The main configuration file

The main element that we use to define and configure the dbt project, including where resources are created, is the main configuration file, dbt_project.yml.

dbt defines the folder that contains the dbt_project.yml file as the dbt project folder and this file contains the essential and general configuration for the project, which can be eventually overridden at more specific levels.

Open it and follow along with our description and changes.

In the first lines, usually 5 to 7, you will find the following:

```
name: 'my_new_project'
version: '1.0.0'
config-version: 2
```