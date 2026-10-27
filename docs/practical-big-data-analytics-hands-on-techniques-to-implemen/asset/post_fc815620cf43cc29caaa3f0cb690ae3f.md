
Since there can be multiple records for each PlotNumber to accommodate change of ownership, we can now define three keys that could uniquely identify each cell of data in each record, as follows:

- Row key: `PlotNumber`
- Column key: The column name
- Timestamp key: `RecordDate`

Each cell in each record in the table will thus have a unique three-value pair that distinguishes it from the other cells.

Databases such as Bigtable, Cassandra, and others employ this method to perform data analysis at scale both expeditiously and efficiently.

Some of the popular columnar databases are listed as follows. Note that there may be repetitions as databases can have multiple NoSQL properties (such as both in-memory and columnar):

| Open source | Commercial |
|---|---|
| Apache Parquet | Kdb+ |
| MonetDB | Teradata |
| MariaDB | SAP HANA |
| Druid | HP Vertica |
| HBase | Oracle Exadata |
| Apache Kudu | ParAccel |
| Apache Arrow | Actian Vector |

## Document-oriented databases

**Document-based or document-oriented** databases became prominent as a means of storing data that had variable structures; that is, there was no fixed schema that each record would fit into all the time. Additionally, the document may have both a structured as well as an *unstructured* part.

Structured data is, in essence, data that can be stored in a tabular format such as in a spreadsheet. Data stored in Excel spreadsheets or MySQL tables all belong to the class of structured datasets. Data that cannot be represented in a strict tabular format such as books, audio files, video files, or social network messages are considered unstructured data. As such, in document-oriented databases, we will primarily work with structured and unstructured text data.

An intuitive explanation of data that can contain both structured and unstructured text can be found in the example of a **phone diary**. Although these have become increasingly rare with the growth of digital data storage, many of us would remember a time when phone numbers were written in pocketbooks. The following image shows how we store data in a phone diary:

![](img/8ededb1e0b69e85687f6e488d1044b79_138_0.png)

In the preceding example, the following fields can be considered as structured:

- Name
- Address
- Tel and Fax

There is a line underneath the Address field where the user can enter arbitrary information, for example, **met at a conference in 2015, works at company abc**. This is essentially a note that the diary keeper wrote when entering the specific information. Since there is no defining characteristic of a free-form field such as this, it could also contain information such as a second phone number, or an alternative address and other information. This would qualify as an unstructured text.

Further, since the other fields are not interdependent, a user may write the address but not the phone number, or the name and phone number but not the address.

A document-oriented database, by virtue of its ability to store schema-free data; that is, data that does not conform to any fixed schema such as fixed columns with fixed datatypes, would hence be an appropriate platform to store this information.

As such, since a phone diary contains a much smaller volume of data, in practice, we could store it in other formats, but the necessity for document-oriented datasets becomes apparent when we are working with large-scale data containing both structured and unstructured information.

Using the example of a phone diary, the data could be stored in a document-oriented dataset in JSON format, as follows:

```
(
  {
    "name": "John",
    "address": "1 Main St.",
    "notes": "Met at conference in 2015",
    "tel": 2013249978,
  },
  {
    "name": "Jack",
    "address": "20 J. Blvd",
    "notes": "Gym Instructor",
    "tel": 2054584538,
    "fax": 3482274573
  }
)
```

JSON, which stands for JavaScript Object Notation, provides a means of representing data in a portable text-based key-value pair format. Today, data in JSON is ubiquitous across the industry and has become the standard in storing data that does not have a fixed schema. It is also a great medium to exchange structured data, and as such is used for such datasets frequently.

The preceding illustration provides a basic example to convey how document-oriented databases work. As such, it is a very simple and hopefully intuitive example. In practice, document-oriented databases such as MongoDB and CouchDB are used to store gigabytes and terabytes of information.

For example, consider a website that stores data on users and their movie preferences. Each user may have multiple movies they have watched, rated, recommended, movies that they have added to their wishlist, and other such artifacts. In such a case, where there are various arbitrary elements in the dataset, many of which are optional and many of which might contain multiple values (for example, multiple movies recommended by a user), a JSON format to capture information becomes optimal. This is where document-oriented databases provide a superior and optimal platform to store and exchange data.

More concretely, databases such as MongoDB store information in BSON format - a binary version of JSON documents that have additional optimizations to accommodate datatypes, Unicode characters, and other features to improve upon the performance of basic JSON documents.

A more comprehensive example of a JSON document stored in MongoDB could be data stored about airline passengers that contains information on numerous attributes specific to individual passengers, for example:

```
{
  "_id" : ObjectId("597cdbb193acc5c362e7ae96"),
  "firstName" : "Rick",
  "age" : 66,
  "frequentFlyer" : (
    "Delta"
  ),
  "milesEarned" : (
    88154
  )
}
{
  "_id" : ObjectId("597cdbb193acc5c362e7ae97"),
  "firstName" : "Nina",
  "age" : 53,
  "frequentFlyer" : (
    "Delta",
    "JetBlue",
    "Delta"
  ),
  "milesEarned" : (
    59226,
    62025,
    27493
  )
}
```

Each entry is uniquely identified by the _id field, which allows us to directly query information relevant to the specific user and retrieve data without having to query across millions of records.

Today, document-oriented databases are used to store a diverse range of datasets. Examples include the use of such the following:

- Log files and log file-related information
- Articles and other text-based published materials
- Geolocation data
- User/user account-related information
- Many more use cases that are optimal for document/JSON based storage

Well-known document-oriented databases include the following:

| Open source | Commercial |
|---|---|
| MongoDB | Azure Cosmos DB |
| CouchDB | OrientDB |
| Couchbase Server | Marklogic |

## Key-value databases

Key-value databases operate on the principle of structuring data as pairs of values corresponding to keys. To highlight the benefits of key-value databases, it would help to revisit the significance of hash maps, a common term prevalent in computer science to specify a unique data-structure that provides a constant-time lookup for key pairs.

An intuitive example for a hash table is as follows:

Consider a collection of 500 books and five bookcases. Each bookcase has five shelves. The books can be placed in an arbitrary order, but that would make it incredibly difficult to find a specific book and you may need to go through hundreds of books before locating the one you need. One method of categorizing the books would be to assign ranges of letters to each of the bookshelves, for example, A-E, F-J, K-O, P-T, U-Z, and use the first letter of the name of the book to assign it to a specific shelf. However, suppose you have a disproportionate number of books that start with the letters A-E. This means that the case assigned for A-E would have a much higher number of books relative to the other ones.

A more elegant alternative could be to assign a value to each of the books and use the respective value to determine which bookcase or bookshelf the book belongs to. To assign a number, a specific value to each book, we could sum up the numbers corresponding to each letter of the title of the book using a range of 1-26 for the letters A-Z respectively:

![](img/8ededb1e0b69e85687f6e488d1044b79_141_0.png)
