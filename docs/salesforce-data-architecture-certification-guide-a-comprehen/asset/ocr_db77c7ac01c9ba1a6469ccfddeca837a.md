+   4. What method is used to speed up the performance of large SELECT SOQL queries when exporting large amounts of data from Salesforce?
+   5. What data API facilitates the asynchronous processing of data in parallel batches?
+   6. Ordering records to be imported by their parent record ID reduces the likelihood of what happening?
+   7. True or false: upsert is considered good for performance when loading large amounts of data.
+   8. PK chunking is a feature of which Salesforce API?
+   9. What’s the first step in any data migration process?
+   10. Why is PK chunking considered faster than a straight *Export All* operation?

# Answers

The following are the answers to the previous questions. How many of them were you able to answer correctly? Check your answers to the previous questions here:

+   1. Large Data Volume
+   2. Primary key
+   3. Data Export
+   4. PK Chunking
+   5. Bulk API
+   6. Parent record locking
+   7. False. Use insert and then update as separate operations.
+   8. The Bulk API
+   9. Define
+   10. Because by breaking the query down into sub-queries, the Salesforce Query Optimizer threshold isn’t reached and therefore the performance of each query is optimal.

# Further reading

See the following resources. For more information on what was covered in this chapter:

+   - Data Export options from Salesforce: https://trailhead.salesforce.com/content/learn/modules/lex_implementation_data_management/lex_implementation_data_export
+   - PK chunking in Salesforce: https://developer.salesforce.com/docs/atlas.en-us.234.0.api_asynch.meta/api_asynch/async_api_headers_enable_pk_chunking.htm