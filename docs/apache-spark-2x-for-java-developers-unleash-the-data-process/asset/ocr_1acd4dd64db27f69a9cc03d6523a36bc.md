# Exploring the Spark ecosystem

Apache Spark is considered as the general purpose system in the big data world. It consists of a lot of libraries that help to perform various analytics on your data. It provides built-in libraries to perform batch analytics, perform real-time analytics, apply machine learning algorithms on your data, and much more.

The following are the various built-in libraries available in Spark:

+   • Spark Core: As its name says, the Spark Core library consists of all the core modules of Spark. It consists of the basics of the Spark model, including RDD and various transformation and actions that can be performed with it. Basically, all the batch analytics that can be performed with the Spark programming model using the MapReduce paradigm is the part of this library. It also helps to analyze different varieties of data.

+   • Spark Streaming: The Spark Streaming library consists of modules that help users to run near real-time streaming processing on the incoming data. It helps to handle the velocity part of the big data territory. It consists of a lot of modules that help to listen to various streaming sources and perform analytics in near real time on the data received from those sources.

+   • Spark SQL: The Spark SQL library helps to analyze structured data using the very popular SQL queries. It consists of a dataset library which helps to view the structured data in the form of a table and provides the capabilities of running SQL on top of it. The SQL library consists of a lot of functions which are available in SQL of RDBMS. It also provides an option to write your own function, called the User Defined Function (UDF).

+   • MLlib: Spark MLlib helps to apply various machine learning techniques on your data, leveraging the distributed and scalable capability of Spark. It consists of a lot of learning algorithms and utilities and provides algorithms for classification, regression, clustering, decomposition,