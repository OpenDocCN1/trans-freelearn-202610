+   - The version: 2 field clearly shows this is an earlier version of the feature
+   - The updatedAt timestamp documents when this version was replaced by a newer one

This document is stored in a separate history collection dedicated to maintaining versions of documents. Storing older versions separately ensures that the main collection remains lightweight while still enabling access to historical data for use cases such as audits, debugging, or comparative analysis.

The document versioning pattern avoids the need for a second write to record “what changed” by separating the current version of a feature configuration from its historical versions. This pattern is best suited for applications that require maintaining an audit trail, tracking changes over time, or supporting rollbacks, such as configuration management systems, content publishing platforms, or any system needing historical data without compromising performance for active workflows.

# Bucketing pattern

For time-series or high-volume data, you can use the bucketing pattern to group related data points into “buckets” to reduce the number of documents, when using a special time-series collection is not possible for some reason.

The following is an example of a timeline cache in the Socialite app that tracks a user’s daily activity:

```
// With bucketing (optimized for fast timeline displays)
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5f2f"),
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  date: ISODate("2023-04-10T00:00:00Z"),
  activities: [
    {
    type: "post",
    postId: ObjectId("5f4e5b6d3c1e2a1b3c4d6f4h"),
    timestamp: ISODate("2023-04-10T10:00:00Z"),
    content: "Just posted a new photo!",
    mediaUrl: "https://socialite.com/images/photo123.jpg",
    likes: 42
    },
  ]
}
```