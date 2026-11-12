Behind the scenes, CommonsHttpSolrServer makes use of Apache Commons HttpClient to interact with the Solr search server. The constructor of CommonsHttpSolrServer accepts URL of the Solr search server as an argument. The `<constructor-arg>` element specifies the value of the constructor argument as `${solr.serverUrl}`, which refers to the `solr.serverUrl` property defined in `solr.properties` file.

+   - Creates a `solr.properties` file in the `SRC_MAIN_RESOURCES\META-INF\spring\` directory. The properties file defines an `executor.poolSize` property, which specifies the thread pool size required by `ThreadPoolExecutor`, as shown here:

```
```
executor.poolSize=10
```
```

The `solr.properties` file also contains a `solr.serverUrl` property, which identifies the URL where the Solr search server is running, as shown here:

```
```
solr.serverUrl=http://localhost:8983/solr
```
```

If your Solr server is running on a different host or port, then change the URL in the `solr.properties` file or use the `searchServerUrl` argument of the `solr` setup command to specify the Solr search server URL.

# Adding domain model data to Solr index and searching Solr documents

Imagine that you want to search for `FlightDescription` instances where the `origin` field is NYC. You can perform this search against the database in which you persist your `FlightDescription` entity instances or you can add the `FlightDescription` instance data into Solr index and search against it. We'll look at how Roo supports adding entity instance data to Solr index, and in the next section, we'll look at how to query that data in Solr search server.

Though there are multiple ways in which you can push data into Solr, Roo makes use of the SolrJ client library to interact with the Solr search server. When the `solr all` Roo command is executed, it adds certain methods (via AspectJ ITD) to JPA entity classes that are fired when an entity is added, removed, or updated. These methods are responsible for adding, updating, and deleting entity data from Solr index using SolrJ client library.

When `solr all` command is executed, the following actions are performed by Roo:

+   - Adds the `@RooSolrSearchable` annotation to JPA entity class that triggers creation of the corresponding `* _Roo_SolrSearch.aj` AspectJ ITD file.

The following code listing shows the `FlightDescription` JPA entity of `flightapp-web` project after `solr all` command was executed:

```
```
@RooEntity(identifierColumn = "FLIGHT_DESC_ID", table = "FLIGHT_DESC_TBL", finders = { "findFlightDescriptionsByDestinationAndOrigin" })
```
```