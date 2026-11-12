Deferring the processing of sharing rules can come in handy when you are loading a massive number of records. You can stop calculating sharing records until you have finished loading the new data or completed a multi-step change of sharing mechanisms across the org.

> **Note**
> You can find out more about this feature at the following link: http://packt.link/TPeWw.

# Query Optimizer

Query Optimizer is a *transparent tool* that works behind the scenes. It evaluates queries (SOQL and SOSL) and routes the queries to the right indexes. As Salesforce is a *multi-tenant platform*, it needs to track its own statistical information to find the best way to retrieve the data. The statistics-gathering process takes place *nightly*, with no user interference required.

# Bulk API

The Bulk API is designed to provide a quick and efficient mechanism for loading or retrieving a vast amount of data into/from a Salesforce org.

The Bulk API functionality is available if it is enabled on your Salesforce org. It is enabled by default for performance, unlimited, enterprise, and developer orgs.

You can write a custom application that utilizes the Bulk API or buy readymade products that do. Most modern ETL tools that come with built-in Salesforce connectors support the Bulk API. Some are even built to automatically switch to the Bulk API when they detect that you are about to load a huge amount of data.

> **Note**
> You can find out more about the Bulk API at the following link: https://packt.link/7RX2x.

# Batch Apex

Batch Apex can be used to develop complex, long-running processes that deal with a huge number of records and therefore require more relaxed *governor rules*.

Batch Apex operates in *batches*, handling one batch of a pre-determined size/number of records at a time. It does this until the entire record set it is operating on is complete.

You can use Batch Apex to build a data archiving solution that runs at a scheduled time (for example, every week). The code could look for records older than a pre-defined age and archive them in a pre-defined mechanism (for example, move them to an aggregated denormalized table).