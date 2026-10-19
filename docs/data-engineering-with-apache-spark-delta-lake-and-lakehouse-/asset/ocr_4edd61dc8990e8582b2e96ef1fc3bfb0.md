+   - Under **Stored procedure parameters**: Add one parameter. Start by clicking New:

+   - **Name:** `goldns` **Type:** `String`
+     **Value:** `@pipeline().parameters.GOLD_LAYER_NAMESPACE:`

![](bbox=[0.11, 0.18, 0.85, 0.39])

Figure 8.16 – Parameters of the stored procedure

11. Now, chain **Get Secret** to **Drop Gold Layer Tables**. Once you've done this, the pipeline should look like this:

![](bbox=[0.21, 0.48, 0.76, 0.57])

Figure 8.17 – Chaining activities – Get Secret to Drop Gold Layer Tables

12. In the **Activities** panel, search for `Stored Procedure`. Now, drag the **Stored procedure** activity to the right-hand panel and provide the following properties:

+   - **Name:** `Create Synapse Credentials.`

+   - Click on **Settings.**

+   - **Linked service:** `Synapse Serverless.`

+   - **Stored procedure name:** `[dbo].[create_synapse_creds]`.

+   - Under **Stored procedure parameters**: Add five parameters. Start by clicking New:

+   - **Name:** `format` **Type:** `String`
+     **Value:** `@pipeline().parameters.EXT_TAB_FILE_FORMAT`

+   - **Name:** `extds` **Type:** `String`
+     **Value:** `@pipeline().parameters.EXT_DATA_SOURCE`