# Adding or editing a query

Once the connection is added (or updated), the next task is defining the queries. Starting from the JDBC Data Source window, the Available Queries panel lists all the queries defined for the current data source. In the top-right corner of the panel, you can see:

+   - A green plus icon (⊕) used to add a new query
+   - A red icon (⊗) used to delete an existing query

A query is generally identified by a name and content. The name of a query can be edited in the Query Name field and the content can be managed in the bottom panel of the window. The content could be a static string or a script. The static query (widely used) is written in the syntax required for the database type (usually SQL). The SQL string can be enriched with some optional parameters defined using a specific notation (you will see the syntax of the notation in the following chapters).

The query scripting is a source code in ECMAScript or Groovy and you can use it to manipulate the dataset, using a programming language. The query scripting is rarely used and we won't treat it in detail, but it's a collection of functions you can develop to make the dataset exactly as you want. The static query can be specified in the Query field of the Static Query tab, and the query scripting can be specified in the Query Script field of the Query Scripting tab.

> *Click on the Template button of the Query Scripting tab to see all the available functions, and documentation on how to develop the dataset using the source code instead of a standard query.*

The pencil icon (✎) in the Static Query tab is useful to launching SQLeonardo Query Builder, a powerful and easy-to-use GUI that lets you query databases without any relevant knowledge of the SQL syntax. Max Preview Rows, if enabled, lets you declare the maximum number of rows retrieved for previewing the data, and the Preview button checks that everything is working as expected and shows a bunch of data results as a data table.