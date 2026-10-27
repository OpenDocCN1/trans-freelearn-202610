
To pick a name for the positions at the ABC Bank, you must consider what the model represents for your project:

- If you consider this the model that represents the positions at the ABC Bank so that it is ready to be reused whenever you need those positions, then it makes sense to consider this the refined model for the concept, and name it with the REF prefix.
- If, instead, you consider this an intermediate step in the process to make that data available in a more reusable way, then you should use the TR prefix.

As often occurs in IT, you might have good considerations that argue for both directions, such as the following ones:

- We know that security symbols and exchange codes are different for each broker, so we will need some master data layer to be able to combine and reuse data in a general way.
- Currently, this is the best representation that we have of such data, and this is the best data that we can expose and use in reporting.
- In reporting, we want to expose only refined data, which can be used with little or no caveats and possibly reused and combined with other data. As a general pattern, we prefer data mart models to depend only on REF or equivalent models, such as AGG or MDD, and only from the refined layer.

We have pros and cons in both directions, and I am sure that you can find more yourself, but the important thing is to make a choice.

I urge you to consider that a project, and especially an agile project, is always changing, and moving forward with a good enough, generally accepted name is better than stalling and not moving forward. Luckily, the dbt tool and our way of working make changing names and code simpler than it was in the past, especially in the REF layer.

In this case, the model is the “most refined” that we have for the moment, even if we think that we will change it in the future, so our choice is to use the REF prefix.

Please go to your dbt Cloud IDE and rename the `POSITION_ABC_BANK` model to `REF_POSITION_ABC_BANK`, and then update the model’s name in the `refined.yml` YAML file so that the description and tests keep working as expected.

Once you have tried the model and tested it, you can commit your change and run the model to deploy it in the database, using the following command:

```bash
dbt run --select REF_POSITION_ABC_BANK
```

Now, you have deployed the new model in the refined layer, which is connected to the STG model.

> **Tip**
>
> Please remember that dbt will not remove the table or view that you might have created with the old name. As we are in our development environment, you can just delete the old object with a `DROP` command to keep your project clean.

One possible alternative is to also create a TR model with the current code and a REF model that, for now, just reads everything from the TR model but, in the future, will evolve to apply the desired MD transformations. This is a good way to lay out practically and tangibly the future architecture for all to see, but it also risks introducing complexity with an extra step that will not be used for a while and potentially forever.

Applying the **You Ain't Gonna Need It (YAGNI)** principle, you should not introduce things (features, models, and any other complication) until they are really needed. Following this principle is essential in big projects, and for this reason, we think that adding a temporary or skeleton REF model would only be a good idea when you already have a good plan on how to develop it, you are ready to do that development in a short time frame, and splitting the model in two would ease the work of the development team.

## Creating the first data mart

In this section, we want to complete our initial architecture by creating the first model in the first data mart of this project. Data marts make up the delivery layer.

![](img/8868d272a75eee8b407a8e0f3434d10f_265_0.png)

*Figure 6.5: The delivery layer highlighted in the Pragmatic Data Platform*

The general plan is to have one data mart for each domain that will use the platform’s data so that we can provide the best dataset for each of them, using the right names, the correct depth and granularity of the data, and making available only the concepts that matter for that domain.

In the context of our simple example, we do not need multiple data marts, so we have decided to create a data mart with the name of `portfolio`, which we have already set up in the main configuration file, `dbt_project.yml`.

To create the data mart in practice, we have to create the `marts` and `portfolio` folders and then put the desired models inside the data mart folder.

Let’s do it:

1. Create the `marts` folder under the `models` folder. You can do this by clicking on the three dots menu of the **models** folder and then picking the **New Folder** entry. You can enter only the `marts` name after `models/`, or do both steps in a single command by entering both levels as `models/marts/portfolio`.
2. Create the `portfolio` folder under the `marts` folder.
3. Create the first data mart model with the name `FACT_POSITION`. You can do it by picking the **New File** entry from the three dots menu of the `portfolio` folder and adding the model’s name at the end of the path, with the `.sql` extension, producing the full name `models/marts/portfolio/FACT_POSITION.sql`.

Our first version of the model will just read all data from the model in the refined layer, as seen in the following code:

```sql
SELECT * FROM {{ ref('REF_POSITION_ABC_BANK') }}
```

4. Once you have saved the model, you can deploy it with the following command:

```bash
dbt run --select FACT_POSITION
```

5. Once you have deployed the model, you can check in your Snowflake database that you now have a new schema named `XX_MART_PORTFOLIO` (replace XX with your personal schema) and, inside that schema, a table named `FACT_POSITION`.

With these steps, we have just created the first table, a fact, in our data mart.

A data mart with a single table is not very interesting, but in this case, our fact is the bare minimum we need to start building some basic reporting, and for us, the path that took us here has been more important than the current final result. Building the report is out of our scope.

For now, our data platform just reflects the latest content of our input.

In the next section, we will add to our platform the ability to store its input and changes over time.

## Saving history is crucial

A data platform that does not store its input is a very fragile platform, as it needs all the input systems to be available at the same time of each run to be able to produce its results.

An even bigger limitation is that a platform without history cannot fulfill many of the requirements that are otherwise possible and expected today by a modern data platform, such as auditing, time travel, bi-temporality, and supporting the analysis and improvement of operational systems and practices.

To us, anyway, the core reason why you should always save the history of your entities is to build a simpler, more resilient data platform. When you split your platform into one part that just adapts and saves the history (the storage layer) and another that uses the saved data to apply the desired business rules (the refined layer), you apply the principles that we have discussed and achieve a great simplification of your data platform.

![](img/8868d272a75eee8b407a8e0f3434d10f_267_0.png)

*Figure 6.6: The storage layer highlighted in the Pragmatic Data Platform*

The question you might have in mind is, “OK, that’s the pros, but what about the cons?” This is a fair question, and surprisingly the answer is that there are no real cons!

All the topics that I will list here are, ultimately, a net positive for the data platform, even if someone might initially see them as cons because they require “extra work”:

- **Your people (tech and business together) MUST understand the data they ingest and be very clear about the IDENTITY of the entities they store.**

  Being forced to decide/discover a primary key might feel like a pain, but the result is much-improved use and interoperability of the data across all the data platforms.

- **You will store the good, the bad, and the ugly.** While you might care only about the good, having it all and being forced to explicitly manage the bad and the ugly will tell your people, especially the business ones, a lot about your operational systems and processes.

- **Storing history takes up more room for your data.** That is true, but in most cases, the rate of change is very low, as only a few of the entities in bigger tables have more than one version, so the amount of extra data is little. Furthermore, the columnar storage of a modern cloud platform greatly reduces the amount of room used.

In any case, the value that you get from the simplification of the data platform, and the extra features that the history enables, is much more than the tiny cost of the extra storage. INSERT is by far the best-performing operation on cloud platforms, so in the advanced section, we will move to insert only history, as snapshots use updates, are shared across environments, and become a deployment issue when you want to change them.
