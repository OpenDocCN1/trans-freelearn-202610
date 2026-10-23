In the next chapter, we’ll explore more of the concepts around how you can mitigate **Large Data Volume (LDV)** concerns through practical examples and how to load massive amounts of data.

# Practice questions

Test your knowledge of the topics covered in this chapter by answering the following questions:

+   1. What is the only data format that’s suitable for SOAP API requests?
+   2. What are the two job types that the Bulk API is used for?
+   3. What type of resource is used to describe the operations that are offered by a SOAP API that can be imported by calling applications?
+   4. How many retries are attempted by a Bulk API Query operation?
+   5. What batch size are records divided into when running a Bulk API Ingest operation?
+   6. What operations are supported as part of a Bulk API Ingest job?
+   7. What interface is extended when writing an Apex batch class?
+   8. What annotation is used for asynchronous Apex methods?
+   9. The Apex `insert` statement, followed by a list of records of a particular object type, is an example of what type of approach when working with the Salesforce database?
+   10. The Apex `Database` class’s insert/update/upsert/delete methods have a Boolean parameter that does what?

# Answers

The following are the answers to the previous questions. How many of them were you able to answer correctly? Check your answers to the previous questions here:

+   1. XML
+   2. Ingest and Query
+   3. WSDL
+   4. 15
+   5. 10,000
+   6. Insert, update, upsert, delete
+   7. Database.Batchable
+   8. @future
+   9. A DML statement
+   10. Determine whether to allow partial processing of records