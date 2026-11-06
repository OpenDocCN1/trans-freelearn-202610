The analytics node, which we’ve identified as having a tag of usage: reporting, can be targeted by specific workloads by using the appropriate read preference configuration, such as in the following example:

```
db.collection.find({}).readPref("secondary", [{ "usage": "reporting" }]);
```

Replica set tags cannot be directly configured in MongoDB Atlas; however, Atlas offers a set of predefined replica set tags that can be used for workload isolation, some of which are as follows:

| Predefined tag name | Description | Example |
|---|---|---|
| nodeType | Node type. Possible values are as follows:
- ELECTABLE
- READ_ONLY
- ANALYTICS | {"nodeType" : "ANALYTICS"} |
| provider | Cloud provider on which the node is provisioned. Possible values are as follows:
- AWS
- GCP
- AZURE | {"provider" : "AWS"} |
| region | Cloud region in which the node resides. | {"region" : "US_EAST_2"} |

Table 5.2: Common predefined tag names for MongoDB replica set members

For a full list of the predefined replica set tags and possible region values available in MongoDB Atlas, see https://www.mongodb.com/docs/atlas/reference/replica-set-tags.

If an application performs complex or long-running operations, such as **extract**, **transform**, and **load (ETL)** or reporting, you may want to isolate the application’s queries from the rest of your operational workload by exclusively directing those queries to analytics nodes. This can be configured based on the preceding predefined tags for an application connecting to a MongoDB Atlas cluster, as follows:

```
mongodb+srv://<db_username>:<db_password>@abc123.mongodb.net/test?readPreference=secondary&readPreferenceTags=nodeType:ANALYTICS
```