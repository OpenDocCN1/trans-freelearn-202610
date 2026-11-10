# Documentum 5.3 Update

Documentum release 5.3 has introduced a new querying approach called an FTDQL query which is similar to a SELECT statement, with the difference that the query is run against the full-text index rather than the Docbase (repository) for performance gains.

In order to learn more about DQL queries, please go through Chapter 26.

# 2.8 API

API commands (also referred to as Server API) are instructions sent to the Content Server by clients via DMCL (Documentum Client Library). Similar to DQL, API commands are used to:

+   - Query, update, and delete objects in Docbase
+   - Create new objects in Docbase

Unlike DQL queries, which can manipulate multiple objects at a time, API commands are meant to be executed on one object at a time.

Example:

```
get,c,0900223280023fc2,object_name
...
```

Result: SampleDocument.xml

Let us break down the API command to explain the example:

+   - get: A Server API method used to retrieve information about a particular attribute value
+   - c: Signifies the current Docbase session
+   - 0900223280023fc2: r_object_id of the object in question
+   - object_name: Name of attribute whose value needs to be retrieved

Note that the arguments to Server API methods are positional and should not include any white spaces.

IAPI is an interactive utility/tool installed along with the Content Server, which allows one to execute Server API methods against a Docbase. Figure 2.9 shows how a sample API command is fired using the IAPI utility.