
Unit Testing focuses on testing a small part of an application at a time. Tests are defined in code, so that they can be run easily and quickly. To ensure they run quickly, true unit tests should not use external resources such as the network or files.

For the code behind an application to be easily unit tested, the design has to allow for the objects to be tested in isolation. For example, all IO may be wrapped into a simple class so that a unit test can be written that uses a fake, or mock, version of the IO class.

This impacts the design but makes sure the code is tested all the way through development. Tests can even be written before the development starts.

## Running unit tests

Unit tests are run like any other Dart command line program from your chosen IDE/Editor or from the command line. For instance, from the Unittest folder the test can be run as `dart bin/main.dart`:

```
00:00 +0: HelloWorldTest
00:00 +1: All tests passed!
```

The default output for the test runner includes color characters (which may not be supported in your IDE or shell):

![](img/52c05b53a56abd53584e4fed738ed509_175_0.png)

## Live Data Collection

```dart
  o = new GeoUpdate(sampleJson);
});
test('Test 1', () => expect(o.features.length, equals(5)));
test('Test 2', () => expect(1, equals(1)));
});
```

Developing tests and the implementation is usually an iterative affair during the development of a project. As the second test in this groups shows, it is easy to put in a placeholder test. Tests help ensure that the final implementation of the code has an upfront design, is loosely coupled, and also provides an easy way to automatically retest a range of functionalities when a change is made.

Contrast running a set of unit tests in a few seconds covering many test cases, taking a few minutes to run a full web application, browsing to a page, entering data, checking the result, and only having covered a single test case.

## Examining the test results

Running the unit tests produces results that show all the tests are failing. This is expected as we have not written the implementation yet.

We are not ending the chapter on a low note! What we have done is considered the design, defined some requirements for the object, and created a test suite to validate the solution as we create it.

## Summary

Serious applications need robustness, and you will now have a grasp of unit testing in Dart to aid in the rapid production of quality software. We have also covered how to log vital operation details, categorized to the correct level, for long-term monitoring of a system.

Connecting Dart to a serious database is critical for many applications, and you can now carry this out using an industrial-strength database system. You now have the skills to connect to a remote web service to collect real data.

In the next chapter, our exploration of database systems will cover extracting the data into our application, and we will mine this constantly updating data store to populate a rich data grid view of the incoming data, which will be updated in real time.

## Live Data and a Web Service

> > Data is a precious thing and will last longer than the systems themselves.
> >
> > — Tim Berners-Lee

For many today, it is hard to imagine that, at one time, most personal and work computers did not connect to a wider network. Not that long ago, a spontaneous network stream of instant notifications to a device in your pocket was unthinkable.

Newsfeeds, streams, and updates are the standard of web applications today. It is not enough for data to be accessible or discoverable. It has to reach out to the user, ideally even before they know they want it.

### Freeing the data

The data that was collected by the data monitor is rather trapped in the database, and while the database system PostgreSQL has a reasonable display in the GUI of pgAdmin, a web page would be more pleasing, and viewable by a wider audience.

Sharing the data to be used by a client? I hope JSON sprung immediately to your mind! The end goal of this phase of the project is a web page that we can visit to view the earthquake information in a friendly manner. The page should update itself smoothly and the data should be made available in a consumable manner for future expansion.

Open the project in the QuakeMonitorDB folder in the sample code for this chapter.

## Reworking the data collector

The iteration of the GeoJSON data collector in the previous chapter was a straightforward capture of data to the database. In order to make the data easier to handle for other applications, the program will be improved to make a table of individual features listed.

## Adding a new data table

The separated features in the database will be stored in a new table named, `dm_quakefeatures`, as follows:

```
CREATE TABLE dm_quakefeatures
(
    qufeat_id text NOT NULL,
    geojson text NOT NULL,
    modified_date timestamp without time zone DEFAULT now(),
    CONSTRAINT dm_qufeat_pkey PRIMARY KEY (qufeat_id)
)
WITH (
    OIDS=FALSE
);
ALTER TABLE dm_quakefeatures
    OWNER TO webquakeuser;
```

The SQL statements for creating the database are found in the SQL folder of the sample code.

## Filtering the data

The DataMonitor class in the file quakemonitordb.dart has an updated fetchData method, which individually stores the underlying features after storing the raw JSON to the database:

```
GeoUpdate update = new GeoUpdate(JSON.encode(dataset));

log.fine("Features to process ${update.features.length}");

for (int i = 0; i < update.features.length; i++) {
    GeoFeature feature = update.features[i];

    await daoGeoJson.storeFeature(feature.id, feature.toJson());
}
```

As the preceding excerpt shows, this is not a complicated object that arranges data into convenient fields.

## Converting the feature to JSON

To convert the object back to JSON, a specific method will be added, called `toJson`, as shown in the following code snippet:

```
String toJson() {
    Map out = {};
    out["properties"] = properties;
    out["geometry"] = geometry;
    return JSON.encode(out);
}
```

The key fields of properties and geometry are added to Map before being passed to the encoding function and returned to the caller.

## Improving the data maintenance

Since there is a new table in the database, we will want to update the maintenance program `maintenance.dart` to clear both tables, as shown in the following code snippet:

```
deleteRecords() async {
    var dbConn;
    try {
        dbConn = await connect(uri);

        var query = "delete from dm_quakefeed";
        await dbConn.execute(query);

        query = "delete from dm_quakefeatures";
        await dbConn.execute(query);
    } catch (exception, stacktrace) {
        log.severe("Exception getting info.", exception, stacktrace);
        print(exception);
    } finally {
        dbConn.close();
    }
}
```

This is carried out by using another `delete` SQL statement, and reuses the existing connection to the database.

## Storing the single feature

The storage of the feature will take two separate queries. The first will use the ID field to check if it is currently in the table. If it is, then the function will exit without storing the JSON string. Let's have a look at the following code snippet:

```
storeFeature(String featureID, String json) async {
    var dbConn;
    try {
        dbConn = await connect(uri);

        var query = """select count(*) as Count
            from dm_quakefeatures where qufeat_id ='$featureID'
        """;
        var results = await dbConn.query(query).toList();

        if (results[0][0] != 0) return;

        await dbConn.execute(
            'insert into dm_quakefeatures (qufeat_id, geojson) values (@
            qufeat_id, @geojson)',
            {'qufeat_id': featureID, 'geojson': json});
    } catch (exception, stacktrace) {
        log.severe("Exception storing Feature.", exception, stacktrace);
        print(exception);
        print(stacktrace);
    } finally {
        dbConn.close();
    }
}
```

The first query returns the number of records that match the ID. The result returned is converted to a list format, and the value is accessed via [0][0] — the first record and first field. If the result is not zero, then the method returns without further action.

The second SQL operation stores the feature in the database by using an insert statement.
