# Monitoring and troubleshooting connections

Effective connection management requires ongoing monitoring and the ability to quickly diagnose problems when they occur. By implementing proactive monitoring practices and having clear troubleshooting approaches ready, you can identify potential issues before they impact your application’s performance and reliability.

MongoDB drivers expose detailed metrics about their connection pools, giving you visibility into how your application is utilizing database connections.

Most MongoDB drivers implement the Connection Monitoring and Pooling (CMAP) specification, though each language driver provides different methods for accessing these metrics. For example, in Node.js, you can use event listeners to monitor connection pool activity:

```
// Node.js example using event listeners for CMAP events
const uri = "mongodb+srv://user:pass@abcdef.mongodb.net/";
const client = new MongoClient(uri);
client.on('connectionPoolCreated', event => console.log(event));
client.on('connectionCheckedOut', event => console.log(event));
client.on('connectionCheckedIn', event => console.log(event));
```

See https://www.mongodb.com/docs/drivers/node/current/monitoring-and-logging/monitoring/#connection-pool-events to learn more about tracking these connection metrics using the Node.js driver.

While driver metrics capture the application’s view of its interactions with the database, MongoDB servers provide complementary metrics about incoming connections from their viewpoint. These metrics can help you understand your deployment’s overall connection utilization across all client applications:

+   - **connections.current**: Number of active client connections to the server. Approaching the maximum indicates potential connection saturation.
+   - **connections.available**: How many more connections the server can accept before reaching its configured limit.
+   - **connections.totalCreated**: Total number of connections created since server start. Rapid increases may indicate connection churn.

You can retrieve these metrics through the serverStatus command in MongoDB Shell or through your preferred MongoDB driver:

```
db.adminCommand({ serverStatus: 1 })
```