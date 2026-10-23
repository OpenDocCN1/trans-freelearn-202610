
The **Add new event** screen can now be seen by following the method in the previous chapter.

The following screenshot is an example of the **Add new event** screen. Take note of the radio button for **Virtual Event**, which automatically gets created from the Boolean value specified in the schema:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_75_0.png)

## 5 Sanity's GROQ Language

In this chapter, we are going to explore GROQ, Sanity's open source query language. We will learn why GROQ was created and how to use GROQ and the Vision plugin to interact with our Sanity Studio dataset. We will also do a comparison between GROQ and SQL, and after that, we will run basic and advanced queries with GROQ.

By learning what GROQ is and how to use it, we can then quickly begin to use it to write custom queries to obtain filtered and sorted data.

We will cover the following topics in this chapter:

- Why GROQ?
- GROQ versus SQL
- Basic queries with GROQ
- Advanced GROQ

### Technical requirements

A web browser and terminal emulator are needed for this chapter.

The code files for this chapter are placed at https://github.com/PacktPublishing/Jumpstart-Jamstack-Development/tree/chapter_five.

### Why GROQ?

The datastore is the most important part of what defines Sanity. Sanity is built for structured content, which is richer than simple two-dimensional tables. Likewise, GROQ is meant to be more powerful and easier to use than traditional SQL, used to query a database. GROQ stands for Graph Oriented Query Language, so, as the name suggests, it is a query language used to query graphs, which are able to represent more complex data structures.

GROQ has four main features:

- **Expressive filtering:** This allows us to build the query using a speakable syntax. If we are looking for a venue within our venues named Will's Pub, then we will simply use the following expression, which equates to where the type is venue and the venue name is Will's Pub:

  ```
  _type == "venue" && name == "Will's Pub"
  ```

- **Joins:** GROQ can join together several documents, combining the result together.
- **Shaping:** You can decide which content fields to include or also rename the field names, reformat the resulting data structure, and even do additional filters within a projection using the values of the parent document.

  Even though this way of obtaining information is quite a paradigm shift for many database developers who are familiar with SQL databases, with experience GROQ can become very easy to use. Additionally, since GROQ is open source, all the code can be examined.
- **Compactness:** GROQ is more compact and less verbose than standard SQL. The syntax is terser in most cases – just as ES6 is terser than ES5.

Sanity Studio's toolbar is displayed at the top of the page found at http://localhost:3333 by typing the following command in the terminal while in the studio directory:

```
sanity start
```

Two of the main tools – the **Dashboard** and **Desk** are automatically installed. To be able to try out GROQ with actual queries against a dataset, we will install a third tool, called **Vision**. Vision gives us a way to type in queries and instantly view the resulting dataset, optionally filtered and sorted. This will help us use GROQ inside of code.

### Installing Vision

To install Vision, take the following steps and use the terminal command line:

1. Inside the `studio` folder, type the following command:

   ```
   sanity install @sanity/vision
   ```

   Upon successful installation, the **Vision** icon should be visible together with the other tools in your local Sanity Studio application:

   ![](img/97ab031f38e2174e2f1b51da18dfa1d0_79_0.png)

   Figure 5.1 – The top navigation

2. Click on the **Vision** icon and a screen will appear that will allow the user to execute GROQ queries:

   ![](img/97ab031f38e2174e2f1b51da18dfa1d0_79_1.png)

   Figure 5.2 – The Vision screen for GROQ queries

In the top navigation, the **DATASET** selector allows the user to select which dataset to use. The light-blue **Run query** button will perform the query. The **Listen** button will activate a mode where changes in the documents from the filter part of your GROQ query are output in real time in the **RESULT** section.

The main body of the screen is divided into three sections. It is very easy to imagine what each section is used for:

- **QUERY** is where the GROQ query will be entered.
- **PARAMS** is where parameter sets that will be passed into the GROQ query can be entered.
- **RESULT** is where the result will be displayed in JSON format. The results may then be expanded or collapsed so that the user can see the overview as well as details.

We have learned how to install the Vision GROQ tool and how to use it. In the next section, we will examine GROQ and compare it to SQL.

### GROQ versus SQL

The compactness of GROQ simply uses an initial asterisk sign to denote `select all documents`, and not a single table by default as in standard SQL. While SQL is a traditional schema database, which means you have to decide the structure of your database in advance, divided into tables. Before storing any data, a programmer must define a table, names, and types of columns inside that table. When adding data to a schema database, a programmer must pass the data in the format declared on each column; any extra data or data not matching the format will not be stored in the database.

Sanity's content model is a schema-less database which means you don't need to define tables and columns. The system stores data as key/value pairs or JSON, basically a one-column table. A programmer will be able to add any data in any format into the database, change the existing data, from a Boolean to a string, for example, and add new data types without declaring a new column.

Now let's explore the differences when querying SQL and GROQ:

| SQL | GROQ |
| --- | --- |
| `select * from table;` | `*` |
| `select * from content_table where type="movie";` | `*[_type == "movie"]` |

A traditional SQL-based database query should have the following syntax:

```
select * from table;
```

Let's look at GROQ:

```
*
```

```
"name": "IT Conference",
"venue": {
  "_ref": "65929c5e-f2df-4483-b2e4-4f10a654adf8",
  "_type": "reference"
},
1: { ... },
2: { ... }
]
```

Note that the results are wrapped in `result`. They are represented by a zero-based array and there are three results, so 0, 1, and 2.

Notice that the id is a Universally Unique Identifier (UUID). This allows for the document to be unique throughout the entire dataset, unlike a database table index, which gets indexed starting with 1, but it is only unique within that single table.

The `_rev` key is the identifier of the revision, or version, of this document.

Since `venue` is a related document, the `_ref` is the reference to the `_id` of the document. Note that `venue` is nested and represents a complete document.

### Selecting all upcoming events

In this query, we are going to select all upcoming events by adding the `dateAndTime` condition:

```
*[_type == "event" && dateAndTime >= now()] | order(dateAndTime asc)
```

We pass the current date value using the `now()` function, which returns the current date. We could also have passed the `dateAndTime` value as a string with the YYYY-MM-DD format.

We are also ordering the result by date. We will show all upcoming events starting from the closest event.

### Selecting all past events

As for upcoming events, we are using the `now()` function to get the current date value and get all past events:

```
*[_type == "event" && dateAndTime < now()] | order(dateAndTime asc)
```

It will order the results by date starting from the closest.

### Selecting upcoming virtual conference and non-virtual conferences

Our previous query returned all events by date but was not filtered by the Virtual Conference field. Our Virtual Conference field in the Event schema is a Boolean. We can add the condition to our GROQ query setting `virtual == true` if the event is virtual or `virtual == false` if the event is not virtual:

- Select virtual events:

  ```
  *[_type == "event" && dateAndTime >= now() && virtual == true] | order(dateAndTime asc)
  ```

- Select non-virtual events:

  ```
  *[_type == "event" && dateAndTime >= now() && virtual == false] | order(dateAndTime asc)
  ```

We have seen how to select all of the fields from the dataset schema. Next, we will learn how to select specific fields to make the result size smaller.

### Selecting specific event fields

The results will depend on where we show the content. We may not need all the fields, but rather only certain fields. With this query, we receive only the `name`, `dateAndTime`, and `body` fields for all upcoming events.

We pass the field names in the projection:

```
*[_type == "event" && dateAndTime >= now()] | order(dateAndTime asc) | {name, dateAndTime, body}
```
