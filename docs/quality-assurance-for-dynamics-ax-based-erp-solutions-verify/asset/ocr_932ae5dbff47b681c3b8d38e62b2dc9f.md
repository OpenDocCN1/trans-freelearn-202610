# Database Design

The database design principles are based on the following considerations:

+   - Minimizing the database calls by caching
+   - Minimizing database transactions
+   - Wise index designing
+   - Using the select statement in an optimum way
+   - Performing transactions in the shortest time possible

# Caching

Database access should be avoided whenever it is not absolutely necessary as retrieving database records from memory is far cheaper and faster. Recording database records in memory is known as caching. The following are the possible type of caching on the server:

+   - Record caching
+   - Entire table caching
+   - Result-set caching

# Record Caching

Record caching is a type of performance enhancement technique in which one or a group of records is retrieved from the memory rather than the database. Retrieving a record from memory rather than database significantly improves the data access. Record caching can be enabled only when the following conditions are satisfied:

+   - The CacheLookup property in the table should be enabled by selecting the values notITTS, Found, or FoundAndEmpty.
+   - The table has a unique index; either use the primary index or the first unique index. The index based on RecId (known as RecId index) does not qualify as a caching index.

The retrieved records can be placed in cache if the following conditions are met:

+   - The table is cached i.e. the above conditions are met.
+   - The select statement used to read the record uses an equal operator (= =) on the caching key.
+   - All the fields in the record are retrieved.