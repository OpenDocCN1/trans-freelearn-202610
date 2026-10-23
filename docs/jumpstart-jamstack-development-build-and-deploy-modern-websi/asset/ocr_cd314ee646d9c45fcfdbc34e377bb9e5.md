There are two basic types of queries:

+   - The all prefix followed by the schema, name, as shown in the following, is used to return multiple records.
+   - The single-record query, which will return only one record.

Let's start by learning about the all query format.

# GraphQL's all query

Consider the following example query:

```
{
  allEvent {
    name
  }
}
```

The preceding query will return the name for all of the events in our dataset. The singular form of event is used here instead of the plural form. The result will be in the same format as the query. Notice that the results are wrapped in a data object. Additionally, the allEvent query is repeated in the result, and the result is in the same format as the query:

```
{
  "data": {
    "allEvent": [
      {
        "name": "IT Conference"
      },
      ...
    ]
  }
}
```

Next, let's learn about the single-record GraphQL query.