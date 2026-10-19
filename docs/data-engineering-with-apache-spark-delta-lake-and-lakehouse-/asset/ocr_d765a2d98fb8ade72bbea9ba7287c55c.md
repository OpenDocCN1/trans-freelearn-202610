+   - **Wait on completion:** Checked:

![](bbox=[0.2, 0.12, 0.76, 0.36])

Figure 9.5 – Configuring Run electroniz_batch_ingestion_pipeline

4. In the Activities panel, search for Execute. Now, drag the Execute Pipeline activity to the panel on the right-hand side:

+   - **Name:** Run electroniz_curation_pipeline

+   - Click on **Settings**.

+   - For **Invoked Pipeline**, choose electroniz_curation_pipeline from the drop-down menu.

+   - **Wait on completion:** Checked

In the **Parameters** section, add the following parameters:

+   - **Name:** STORAGE_ACCOUNT **Type:** String
+   **Value:** @pipeline().parameters.STORAGE_ACCOUNT

+   - **Name:** BRONZE_LAYER_NAMESPACE **Type:** String
+   **Value:** @pipeline().parameters.BRONZE_LAYER_NAMESPACE

+   - **Name:** SILVER_LAYER_NAMESPACE **Type:** String
+   **Value:** @pipeline().parameters.SILVER_LAYER_NAMESPACE

+   - **Name:** STORE_SALES_FOLDER **Type:** String
+   **Value:** @pipeline().parameters.STORE_SALES_FOLDER

+   - **Name:** ADLS_FOLDER **Type:** String
+   **Value:** @{formatDateTime(utcnow(), 'yyyy')}/@{formatDateTime(utcnow(), 'MM')}/@