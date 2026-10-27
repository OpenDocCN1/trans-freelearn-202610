```
dbResults.forEach((record) {
    results.add(record[0]);
});

return results;
} catch (exception, stackTrace) {
    print(exception);
    print(stackTrace);
} finally {
    dbConn.close();
}
}
```

The results are returned as a list of records, and each record returned is a List object. This means that dbr[0] needs to be specified, as we only require the string in the first field.

# Discovering the API

The rpc package provides built-in discoverability for the API. This is provided as a JSON definition of the service that is accessed from the API server:

```
http://127.0.0.1:8080/api/discovery/v1/apis/quake/v1/rest
```

This definition would be consumed by development tools to produce wrapper objects and supporting code in the required language. Let's have a look at the output in the following screenshot:

![](bbox=[0.24, 0.57, 0.76, 0.83])