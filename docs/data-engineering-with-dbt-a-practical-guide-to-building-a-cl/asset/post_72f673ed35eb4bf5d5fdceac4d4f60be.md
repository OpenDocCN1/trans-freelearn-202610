
## Data Sources

Figure 12.29: Data source freshness report, by system

The source freshness report provides a quick and easy way to check if there are issues with the incoming data and, coupled with the ability of dbt to send alerts if a job fails, send an alert when the SLA for a table is not met, raising an error to the responsible person/team. It’s as simple as creating a job that only does the freshness check and configures who needs to be alerted upon failure:

Figure 12.30: The freshness report per system

The report for our sample project shows that we have two systems defined and each has a table defined. The **abc_bank** system has some stale data, while for the **seeds** system, all data is fresh.

You can set the SLA in the YAML file where you declare the source system by setting the threshold level and naming the timestamp column:

```yaml
sources:
  - name: seeds
    freshness:
      warn_after: {count: 3, period: day}
      error_after: {count: 5, period: day}
    loaded_at_field: LOAD_TS
```

As shown in the preceding code, you can provide a warning level and an error level, and you have to name the timestamp column to check against the current time.

Like most dbt configurations, you can set the general SLA at the system level and override both the threshold and timestamp column for each source table.

If you set a system-wide SLA you can disable the freshness check for the tables that do not need it (think reference tables that change seldom) by setting the freshness to **null**, as we did for the t2 table in the following snippet:

```yaml
tables:
  - name: t1
  - name: t2
    freshness: null   # no freshness for this table
```

The data source freshness report is accessed as the last entry in the **Deploy** menu.

![](img/8868d272a75eee8b407a8e0f3434d10f_441_0.png)

Figure 12.31: The Deploy menu with the Data Sources entry

The freshness report is also available to non-developer users that access dbt with a reader license, making it available to business users, together with the generated documentation.

## Exposures

Exposure adds a terminal node in the dependency graph to describe an external dependency upon some specific dbt models.

At the core of an exposure, there is a list of the dbt models that the exposed dependency depends upon.

The main benefits of exposures are as follows:

- A clear name to select the listed models that the external feature depends upon, to apply dbt commands simply and consistently
- The ability to add manually curated documentation to the doc site for the report and direct access to the models upon which the exposure depends

The following code creates an exposure for our simple report made up of our only fact and our only dimension:

![](img/8868d272a75eee8b407a8e0f3434d10f_442_0.png)

Figure 12.32: Code to define an exposure for our sample project

Let's analyze it:

- You create an exposure by providing a name, which is therefore mandatory.

  The name must be in camel case, as it can be used as any dbt node name.

- You also need to provide the type of external dependency.

  The external dependency can be anything, but the available types are `analysis`, `application`, `dashboard`, `ml`, and `notebook`.

  The type is used to organize the exposures in the documentation site, where they are grouped by type.

- The last mandatory field is the `email` field of the report owner, while the owner’s name is optional.

- The most important thing to provide is the `depends_on` list.

  While optional, it is the main reason to create an exposure, unless you just want a page in the docs that you can manually edit.

  You can use both the `ref` and `source` functions to build the list, but you will rarely need the `source` function.

  In the list, you should just list the final models that the external feature depends on; the other models that are needed to build them will be known to dbt thanks to the node's DAG. In the preceding screenshot, we listed two models in the data mart layer and dbt shows the full lineage graph.

- Another key component of an exposure can be the description.

  While you can just enter some simple text, it becomes a very powerful way of providing easy-to-use and visually compelling documentation when used with Markdown doc blocks.

- The exposure node will now appear in all the lineage graphs, both in the IDE and in the doc site with the generated documentation.

The result of the previous exposure definition is shown in the following screenshot, which has been enhanced by the use of a Markdown doc block to provide a nicer description of the report's content:

Figure 12.33: Generated documentation for an exposure

In the documentation page generated for the exposure, you can see the details that point to the owner, the description that has been enhanced in its appearance by the use of a Markdown doc block, and the list of models, which are one click away.

The lineage graph is also just one click away, showing the same layout as seen in the screenshot of the IDE.

The exposures have a dedicated menu in the left navigation, with the exposures grouped by type.

## Markdown documentation

When talking about exposures, we showed the result of using a Markdown doc block. What are we talking about?

In dbt, you can declare a description field in most elements described in YAML files. Whenever you can enter a description, you can also define all or part of it using a doc block.

A doc block is the definition of a docs Jinja object with a unique name, which contains text that is interpreted as Markdown when visualized in the doc site:

```
{% docs exp_portfolio_report %}
# Portfolio report
The Portfolio report features are:
* list all positions by account
* provide up to date valorization of positions.
  The value is available
  * in the position currency, depending on the exchange
  * in the account base currency, as set by the owner
{% enddocs %}
```

The preceding doc block that we used in the exposure example must be inside a file ending with .md. It can be in the same folder as your models, with the name that you prefer. More than one doc block can be in the same file.

To use a doc block, you can use the doc function, along with the block's name:

```
description: |
  The basic portfolio report. Positions by account.
  {{ doc('exp_portfolio_report') }}
```

The preceding description mixes and matches normal text with the doc function, using the | YAML block notation that stands for “literal” interpretation of the multiline YAML string.

This is important as the empty line matters in Markdown, and if you would instead use the > YAML folded style, the new line would be replaced with a space, changing your Markdown and therefore the rendered output.

You can also use the doc function directly in a description field for a table, a column, or anywhere possible, but in that case, you might have to quote the doc function:

```
description: "{{ doc('exp_portfolio_report') }}"
```

Whenever you quote a Jinja block, use the opposite type of quotes concerning the ones that you use for the string with the name of the doc block.

Consider that you can use Markdown directly in the description, without the need to create a doc block. This is useful when you want to include links, as shown in the following example:

```
description: "See [the docs](http://www.docs.com)"
```

Providing easy-to-use and pleasant-to-read documentation improves the ability of your power users to make the most out of the dataset that you will prepare. dbt allows you to provide the documentation as you see fit.

## Summary

Congratulations, you have completed the second part of this book and now know all the core dbt functionalities and how to use them!

In this chapter, you learned how to deploy and run your projects and how to use the advanced automation features that allow you to extend dbt to perform all the tasks that you need.

You are now familiar with dbt functionalities and their extensive documentation features, and you can use them to improve the value of your projects.

In the next chapter, *Moving Beyond Basics*, we will start introducing more advanced data engineering topics, such as identity and master data management or saving the changes of your entities, and the dbt functionality that we use to handle them.
