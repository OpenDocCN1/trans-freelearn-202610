
It will be created in the schema that you named in the wizard when you did the Snowflake configuration when creating this project. I used RZ and we suggested using your initials.

It also is the same schema where the initial default project was created when we used `dbt run` to test that everything was configured correctly.

To deploy the code in your DEV environment, you will run the commands manually, as it is common to add one model at a time and change it a few or many times before being happy with it, not wanting to run everything at every change. By running manually, you can run and test what you feel is right at any moment.

We will see that to deploy in a deployment environment, you will use a job – that is, a sequence of dbt commands. You will define it once and run it every time that you want to deploy and run that environment.

The nice thing is that you can define multiple jobs for each environment so that you can have a job that runs everything and another one that just loads the data that changes hourly and only refreshes whatever depends on that data.

## Committing the first dbt model

We have now generated the model in our database by running the needed dbt command, but the code of the model only exists on the dbt Cloud server and is not in our Git repository.

To add the current version of the code to our Git repository, we must commit the changes.

You can do that very easily in dbt Cloud, as it has a guided Git workflow that allows you to easily create a Git branch, then commit the changes while you develop and finally, open a **Pull Request (PR)** to merge that development into the `main` or `master` branch.

To commit the current code for the project, models, and configs, click on the green **commit...** button in the top-left-hand corner of the dbt IDE and then enter a commit message in the popup that will open up. Clicking on the **commit** button will send the code to Git as a new commit.

Remember to save all your open models and config files before completing a commit, as only what is saved will be committed. In any case, rest assured that the dbt IDE will warn you if you have unsaved files.

Now that you have committed them, your changes are saved also to your Git repository, and most importantly, are now available for everyone else that can access the repository.

After each commit, while the code on your IDE is not changed since the last commit, you can open a PR to share the code with your colleagues and start to gather feedback or request a final approval to merge it back into the `main` branch. We will come back to collaboration in later chapters.

## Configuring our project and where we store data

The schema that you define in your dev credentials is the schema where your development environment will exist and, if we do nothing, everything we create will be generated in that schema.

Note that the schema for our DEV environment or a deployment environment is often referred to in the dbt documentation as target schema and it is available in the code as {{ target.schema }}.

Over time, having everything generated in the same single schema will become chaotic, and because one of our main goals is to create and maintain an environment that makes life easier for the developers, let’s fix it and start creating a bit more order by configuring different parts of the dbt project to be created in different schemata in our environment.

The problem is that our environment is defined as a schema and, in a database, you cannot have a schema nested into another schema, so we cannot create multiple schemata inside the target schema for our DEV environment.

To overcome this problem, dbt will generate the tables and views in multiple schemata that have the name composed by the name of our target schema followed by the name of the custom schema that we have defined for the specific content, with the final name of the schema having the following layout: <target_schema>_<custom_schema>.

As an example, if my target schema is RZ and the content is configured to go into a custom schema named REFINED, then the actual schema name when the content is deployed in my environment will be RZ_REFINED. The same content deployed to an environment that has a target schema of PROD will be created in the PROD_REFINED schema.

To recap, remember that you define the target schema where you define the environment, usually along with the connection details, while you configure the custom schema in the main dbt config file, or you override it along the project, as with any other configuration.

## The main configuration file

The main element that we use to define and configure the dbt project, including where resources are created, is the main configuration file, dbt_project.yml.

dbt defines the folder that contains the dbt_project.yml file as the dbt project folder and this file contains the essential and general configuration for the project, which can be eventually overridden at more specific levels.

Open it and follow along with our description and changes.

In the first lines, usually 5 to 7, you will find the following:

```
name: 'my_new_project'
version: '1.0.0'
config-version: 2
```

It is a good practice to change the name to something meaningful, so we will name it `portfolio_tracking`.

You can change the version to `0.1.0` and update it when you do minor releases or leave it as it is and just update it when you do major releases. We will leave it unchanged.

Do not change `config-version`, as that specifies the version of the definition of the configuration to use and version 2 is the one in use now.

Moving down, you find the profile name to use around line 10, as shown here:

```
profile: 'default'
```

This is not used in dbt Cloud, but it is used in dbt Core to select which connection profile to use from the many that can be defined in the connection configuration file.

After this, there are the definitions of the paths where dbt finds the different types of objects, puts the generated code, and what to clean up if you run a `dbt clean` command.

This is usually in lines 15 to 25, as shown here:

```
model-paths: ["models"]
analysis-paths: ["analyses"]
test-paths: ["tests"]
seed-paths: ["seeds"]
macro-paths: ["macros"]
snapshot-paths: ["snapshots"]
target-path: "target"  # directory for compiled SQL files
clean-targets:         # directories to be removed by `dbt clean`
  - <target>
  - <dbt_packages>
```

Do not change these, as they are fine unless you have very special needs.

The last part in the default config file is about configuring models and other dbt objects, as we will see in a second. Over time, we might add configurations for other elements such as seeds, snapshots, variables, sources, tests, and any other feature that we want to configure globally for the project.

The model configuration still reflects the old name and the example project, so let’s change it to suit our current needs, as exemplified here:

```
models:
  portfolio_tracking:
    +materialized: view
    refined:    # Applies to all files under models/refined/
      +materialized: table
      +schema: REFINED
```

You can find the initial version of this source file in our repository in the `05_05_v1__dbt_project.yml` file inside the materials for *Chapter 5* in the `dbt` folder.

Let’s review what we have configured:

- Under the general `models:`, a label that applies to all models from all projects, we have nested the name of our project, `portfolio_tracking:`, to only configure the models of this project.
- Under our project, we have added the model-specific `materialized` configuration with a value of `view`, as we want that our models to be created as views by default.
- Under our project, we have then named the `refined` folder, which does not yet exist, but we can already declare configurations for it and its subfolders.
- Under the `refined` folder, we have declared the model-specific `materialized` configuration again with a value of `table`, as we want the models that are placed under it to be materialized as tables.
- Under the `refined` folder, we have also declared the `schema` configuration with a value of `REFINED`, as we want that the models under it to be created in a schema with the custom schema part of `REFINED`.

We have added a + symbol in front of the configuration keywords, like `schema`, to make it easier to distinguish between them and folder names that are used to hierarchically apply the configurations. Note that + is optional and used only for better readability.

To recapitulate, if we do not define any custom schema for our models, they will be created in the target schema, which is the one that we have defined in the connection configuration. If we define a custom schema, the models will be created in a schema with the name composed by the target schema and the defined custom schema; this ensures that each developer or environment will have its database objects created in schemata with different, unique names.

## Re-deploying our environment to the desired schema

After our first deployment, we added the configuration to be able to generate *refined models* into a custom `refined` schema.

If we now run the `dbt run` command again… nothing changes.

Why? Because we have configured models in the `refined` folder to go to the `refined` schema, and our `POSITION_ABC_BANK` model is in the `model` folder, not in the `model/refined` folder.
