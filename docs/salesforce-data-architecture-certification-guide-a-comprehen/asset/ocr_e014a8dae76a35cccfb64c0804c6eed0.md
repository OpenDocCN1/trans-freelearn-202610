# Extract

Extracting data involves taking the defined source data from source systems. Therefore, to extract data effectively, it is necessary to have the source systems, as-is data models, and any other relevant information well defined and understood. It may be desirable to combine all extracted data into a staging area if any cleansing, de-duplication, or enrichment activities are to be done once all the data has been extracted.

# Transform

Once the data has been extracted, or after the extraction phase of a fully automated **Extract, Transform, Load (ETL)** process, it is usually necessary to transform the data before it can be loaded into the target system. For this to be successful, it is necessary to know about any entity mappings, field mappings, and other rules, conditions, or logic to be applied at transformation time.

# Load

We’re now at the stage of the process where we can load data into the target system. With all of that definition and careful attention paid to preparing the data that’s being loaded, we can choose a tool to use for the loading activity, such as the humble Salesforce Data Loader. You might be surprised at just how much can be automated when scripting Data Loader to run on the command line or as part of a wider automated process. We also need to consider acceptable load performance. We may have millions of records to load, so doing that in an acceptable time frame may differ, depending on the parameters of the data migration exercise, client industry, and so on. For example, I oversaw the data migration process for a bank, which naturally could only take place (and needed to be completed) when the bank wasn’t open for business. Luckily, we tested the process several times, so that was a relatively straightforward Friday evening in terms of getting the data loaded promptly.

# Test

There are generally two types of testing applicable to data migrations:

+   - Testing to specification
+   - Testing for intent

Simply put, testing to specification involves ensuring the right amount of data is loaded. For example, the extract gave us 1,000 company records, so we loaded 1,000 Account records into Salesforce. If the load count was 990, then 1% of the data hadn’t loaded in and we needed to investigate why. This should ideally be identified and rectified as part of testing, but I appreciate that on the day of the production data migration, this may not always be the case.