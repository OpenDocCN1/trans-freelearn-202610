The pattern that we propose here uses the current version in the history as the baseline, comparing the current from history with the first (oldest) of the inputs for each instance (key) and then the remaining inputs in their order from the input.

This demands having a few easy-to-achieve but important conditions:

+   - The order of the versions must be stable. You need to provide a sorting expression to ensure that the versions are evaluated in the right order.
+   
+     The most common situation is to have some effectivity or last modified timestamp that provides the version order, but anything that produces a sort without multiple rows with the same rank will work, including sorting on the ingestion filename and row number.
+   
+     After all, if you cannot establish an order for the versions that you receive, how can you tell which one is the current/most recent? This might be a sign that you are missing something.
+   
+   - To have a lean, well-ordered sequence of versions, the input must only contain versions that are aligned or more recent than the current version from the history.
+   
+     If your input contains versions older than the current from the history, the history will contain the full sequence of versions in the right order, but the versions before the current one from the history will be inserted again without any need, generating rows that are duplicates except for the load timestamp.
+   
+     The natural solution is to use a high watermark column to filter the data incoming from the STG model based on the content of the history table.
+   
+     This pattern is well suited for ingesting delta loads but can work equally well with full exports, so long as you can have a high watermark that is part of the business information, such as a last modified or an effective timestamp, and not some loading metadata such as an ingestion timestamp.
+   
+     In this pattern, the high watermark is implemented at the table level, implemented as max of the given column. It is a common situation and provides good performance, but the pattern could be modified to have a per-key level.
+   
+   - Another important consideration is late arrivals.
+   
+     Data is added to the history table considering the arrival timeline, so input data is considered arrived at the same time or after the previous load.
+   
+     Versions for each key will be compared with the “previous” version based on the provided sorting expression. The first input is compared to the current from history; the others are compared to their predecessor in the input.
+   
+     In many cases, it is possible to architect the HDIFF column, the sort expression, and the load timeline to store data arriving late with the acceptance that some data might be stored twice, but you can read data out of the history table correctly.
+   
+     If you have multiple timelines and for your use case, it is not good enough to compare the input with the most current record in the history table based on one timeline (load timestamp or another), this pattern is not suitable for you.