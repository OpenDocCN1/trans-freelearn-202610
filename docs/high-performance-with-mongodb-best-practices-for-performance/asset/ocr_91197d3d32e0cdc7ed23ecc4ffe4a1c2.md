These considerations also influence whether to model data using large or small documents, which affects how efficiently your application can read, write, and update data.

# Small versus large documents

Understanding how MongoDB stores documents on disk is crucial for optimizing performance. MongoDB’s WiredTiger storage engine, which is covered in more detail in Chapter 7, Storage Engines, uses a page-oriented architecture, where documents are stored on pages that are loaded into memory as needed.

Here is why this matters:

+   - **Minimum page size:** WiredTiger has a minimum uncompressed page size of 32 KB. Documents smaller than this threshold (under 32 KB uncompressed) share a page with other documents, allowing multiple small documents to be retrieved in a single page read.
+   - **Related document fragmentation:** If related data is split across multiple documents, your application ends up making multiple queries to retrieve the complete dataset. Additionally, these documents may reside on different pages, requiring more I/O operations to retrieve all related data.
+   - **Write amplification:** When large documents are updated, even with small changes, the entire document must still need to be written to disk during checkpoints. The larger the document, the more likely it is to cause larger than necessary writes.

Understanding this leads us to our next important principle: *high read velocity workloads benefit from embedded document patterns.* This is because related data is stored in one place, on one page. On the other hand, for high write velocity workloads, where you’re frequently updating small portions of data, multiple document patterns may be more efficient.

# Common myths

Let’s bust some common misunderstandings about MongoDB schema design:

+   - **Denormalization:** Denormalizing data does not always mean *more storage* or *data duplication*. In some cases, embedding related data in one place lowers overall space usage and reduces the number of required indexes. For example, embedding line items within an order document eliminates the need for order ID indexes in both the orders and line items collections, and can help reduce the total data size. Embedding frequently accessed domain data that rarely or never changes will also reduce additional lookups and improve overall performance.