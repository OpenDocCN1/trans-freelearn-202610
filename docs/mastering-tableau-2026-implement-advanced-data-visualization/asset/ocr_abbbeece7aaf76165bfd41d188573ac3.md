![](bbox=[0.362, 0.076, 0.746, 0.553])

Figure 3.52: Save to external database

Since the Tableau release 2022.3, you can write back to CRM Analytics too. To do this, you need a Salesforce account and the according license. Therefore, we will not discuss this option further, but if you are a Salesforce Analytics user, check this out!

Since 2024, Prep has supported more sophisticated write-back operations through an elegant workaround. While Prep doesn't have a native UPSERT button (update existing rows, insert new ones), you can achieve this using temporary tables and custom SQL.

Now, you might think When would I need this?

Imagine you're exporting customer feedback from a survey platform to your data warehouse. People sometimes edit their responses after submitting them. A simple incremental refresh would miss these edits, and a full refresh is inefficient. UPSERT solves this, updating existing rows while inserting new ones, giving you the best of both worlds.