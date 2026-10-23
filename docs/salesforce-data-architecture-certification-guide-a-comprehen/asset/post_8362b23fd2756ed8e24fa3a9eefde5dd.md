
Now that we’ve looked at LDV mitigation methods, let’s look at the various methods for archiving data. As we covered earlier in this chapter, optimally, we only have the right, relevant data on the platform. With that said, let’s explore the strategies for data archiving.

## Data archiving strategies

Just as we have data on Salesforce for our users, there are situations where data needs to be archived off the platform. This may be due to various reasons, such as regulatory compliance (where certain data needs to be retained), or to keep the optimum amount of data in the platform (such as only the data that is being used being kept on the platform, and automatically archiving data over a certain age). Luckily, various options are available for archiving Salesforce data, such as using on-platform solutions such as big objects or storing data off-platform in an external system or data warehouse. We’ll take a look at these options in the following subsections.

## Big objects

As covered in Chapter 2, Data Modeling and Database Design, **big objects** are used to store and manage huge amounts of data (up to 1 million records by default, though this can be scaled up at an additional cost to tens of millions or even billions of records).

Big objects provide consistent performance no matter how many records are loaded into them, which is useful in auditing an archiving use case that big objects are suited to. Anything that requires consistent performance when working with hundreds of millions or billions of records is the use case that big objects are designed for.

Big objects work using a separate, distributed database to the main Salesforce platform, and are not transactional. As a reminder, sharing isn’t supported on big objects other than object and field permissions. Additionally, this means that automation (for example, workflows, flows, and triggers) is not supported on big objects.

## Data Loader

The humble Salesforce Data Loader can be used for both importing and exporting data. This will be covered in more detail in the next chapter, Chapter 7, Data Migration, but for now, we know that the Salesforce Data Loader supports getting data in and out of Salesforce using two different modes, **Serial** and **Bulk**, which support different batch sizes and concurrency options. While Data Loader is generally a manually invoked tool, it can be used as a simple solution for archiving data by exporting Salesforce data as one or more CSV files that can be stored or ingested off-platform, such as on a local server, shared drive, or data warehousing tool.

## ETL

**Extract, Transform, Load (ETL)** tools can be used to automate the archival of data using the Salesforce REST, Partner, and/or Bulk APIs. As part of a regular nightly job, an automated process or scheduled job from a middleware solution such as **MuleSoft** can run, whereby a query is issued to retrieve data that meets the archival criteria since the last successful run of the job. This data can then be saved to an offline data store or inserted into an **Enterprise Data Warehouse (EDW)**.

## Summary

In this chapter, we dug deep into the data architecture and its potential pitfalls and mitigation strategies. Understanding why data skew happens helps us design better parent/child record ownership strategies. Also, understanding the way Salesforce uses indexes ensures that we can create reports, list views, and build queries that work with the constraints of the multi-tenant architecture nature of the platform, not against them. Due to this, we looked at LDV issues and mitigation strategies, understanding how concepts such as selective filter conditions and skinny tables can be used to ensure we can work with our large amounts of data effectively.

Next, we turned our attention to data archiving strategies and the various options we have at our disposal, ensuring that we are keeping to regulatory requirements for data retention and archival (if appropriate) and ensuring that we only use the data that is relevant to our users.

In *Chapter 7, Data Migration*, we’re going to build upon our knowledge of how the data on the Salesforce platform is indexed and organized so that we can start building effective strategies to get data in and out of the platform.

## Practice questions

Test your knowledge of the topics covered in this chapter by answering the following questions:

- 1. What does LDV stand for?
- 2. Which type of Salesforce object is used to store 1 million or more records?
- 3. One account that contains over 10,000 child records is known as what?
- 4. More than 10,000 records looking up to a single parent record is known as what?
- 5. Utilizing data hosted on an external system is a technique known as what?
- 6. What can be used to partition data and reduce the number of records returned by SOQL queries and reports?
- 7. What can be used to avoid joins within queries to data held within a single object and speed up read-only operations?
- 8. More than 10,000 child records belonging to the same parent record is known as what?
- 9. Specify the type of index that's used in the following scenarios:
  - When a SOQL query is executed against object data where that object contains 400,000 records, and the filter matches 40,000 or fewer records
  - When a SOQL query is executed against object data where that object contains 15 million records, and the filter matches 333,333 or fewer records
- 10. Can custom indexes be created on multi-select picklists?

## Answers

The following are the answers to the previous questions. How many of them were you able to answer correctly? Check your answers to the previous questions here:

- 1. Large Data Volume
- 2. Big object
- 3. Account skew
- 4. Lookup skew
- 5. Data virtualization
- 6. Divisions
- 7. Skinny tables
- 8. Ownership skew
- 9. Custom index
- 10. False

## Further reading

See the following resources for more information on what was covered in this chapter:

- *Salesforce Trust:* https://trust.salesforce.com
- *Search Architecture:* https://developer.salesforce.com/docs/atlas.en-us.salesforce_large_data_volumes_bp.meta/salesforce_large_data_volumes_bp/ldv_deployments_concepts_search_architecture.htm
- *Indexes:* https://developer.salesforce.com/docs/atlas.en-us.salesforce_large_data_volumes_bp.meta/salesforce_large_data_volumes_bp/ldv_deployments_infrastructure_indexes.htm
- *Skinny tables*: https://developer.salesforce.com/docs/atlas.en-us.salesforce_large_data_volumes_bp.meta/salesforce_large_data_volumes_bp/ldv_deployments_infrastructure_skinny_tables.htm

## Data Migration

Data migration is essentially the process of taking data from one or more systems, performing operations upon it (such as cleansing, transformation, and enrichment), and then subsequently loading that data into one or more systems.

Typically, when implementing Salesforce, data migration is treated as a discrete workstream with questions to answer. How do you get good-quality data into the Salesforce Platform? How do you ensure loading large amounts of data is smooth, reliable, repeatable, and timely? How do you effectively export data from the Salesforce Platform? What’s the right set of steps to follow when importing or exporting data to or from the Salesforce Platform? Where do you begin?

In this chapter, we will learn the methods available to import data into the Salesforce Platform, including tuning and optimization techniques. Then, we’ll explore some of the concepts and processes that need to be considered and understood when undertaking any large data movement in or out of the Salesforce Platform. We’ll also learn how to effectively export data from the Salesforce Platform.

Data migrations are needed due to several reasons, such as when a new system (such as Salesforce) has been implemented to serve as a single customer view or a company may have acquired another and is merging data into another system. Another example is when data is regularly purchased or acquired, and that data needs to be migrated into a company CRM. Therefore, data migrations are an important, ongoing process.

Data migrations are seen by some as big, scary, and best left to someone else, but once the concepts are understood, migrating data in or out of the Salesforce Platform needn’t be feared. Data migration accounts for 15% of the exam outline. By understanding the material in this chapter, you will be equipped to understand the process, concepts, and intricacies surrounding data movement and the Salesforce Platform.
