
By precomputing this data and embedding it into the document, queries related to engagement statistics and audience demographics can be executed quickly, without requiring access to raw event logs or other related datasets.

This pattern eliminates the need for complex aggregation queries during read operations. Note that this increases the cost of what would normally be just a read or a write by adding an additional write; to avoid this cost, you can queue the additional write to be performed as an async write rather than making it required during the original operation.

## Schema versioning pattern

When applying the schema versioning pattern, you include a version field to track schema changes over time, allowing for smooth transitions during schema evolution.

The following example includes two sample user documents from the Socialite database:

```
// New document with schema version 3
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  schemaVersion: 3,
  username: "jane_smith",
  name: { // In schema v3, we split name into first/last
    first: "Jane",
    last: "Smith"
  },
  email: "jane.smith@example.com",
  // ... other fields
}

// Older document with schema version 2
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  schemaVersion: 2,
  username: "john_doe",
  fullName: "John Doe", // In schema v2, we used a single name field
  email: "john.doe@example.com",
  // ... other fields
}
```

Let’s go over this example to understand how the schema versioning pattern has been applied here:

- **Version identification:** The schemaVersion field explicitly indicates which version each document conforms to, enabling application-level logic to handle variations in the schema during queries, updates, or reads.
- **Schema evolution:** In the example, the two documents are associated with different schema versions. For version 3, the name is stored in two fields (first and last), while in version 2, the name is stored as a single fullName field.

This pattern allows your application to handle documents with different schema versions gracefully. It may not be necessary to use a schemaVersion field unless it’s needed for schema validation or parts of the application cannot make do with just using the absence of a field as an indication of the schema version.

## Polymorphic pattern

The polymorphic pattern stores similar but not identical entities in the same collection for flexible querying.

The following example includes documents from the Socialite database that contain activity feed data, including post, following, and group join activity:

```
// Post activity
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  activityType: "post",
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  timestamp: ISODate("2023-04-10T15:30:00Z"),
  content: "Just posted a new photo!",
  mediaUrl: "https://socialite.com/images/photo123.jpg"
}

// Follow activity
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7b"),
  activityType: "follow",
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  timestamp: ISODate("2023-04-10T16:45:00Z"),
  targetUserId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e8c"),
  targetUsername: "alex_wong"
}
```

```
// Group join activity
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e8d"),
  activityType: "group_join",
  userId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  timestamp: ISODate("2023-04-10T17:15:00Z"),
  groupId: ObjectId("5f4e5b6d3c1e2a1b3c4d5e9e"),
  groupName: "Photography Enthusiasts"
}
```

Let’s go over this example to understand how the polymorphic pattern has been applied here:

- **Shared collection:** All activities are stored in the same collection. Each document represents a distinct activity type but shares common fields such as `_id`, `activityType`, `userId`, `timestamp`, and additional fields specific to the activity.
- **Type-specific fields:** The `activityType` field specifies the type of activity (`post`, `follow`, or `group_join`). Based on this value, type-specific fields are included in each document. For example, a post activity includes `content` and `mediaUrl`.

Now, instead of maintaining separate collections for each type of activity, queries can target the single collection using `activityType` as a filter.

This pattern allows for efficient querying across different types of activities while maintaining type-specific fields.

## Archive pattern for storage optimization

In this section, we’ll look at the archive pattern which is beneficial for storage optimization. This pattern helps reduce active storage costs by moving infrequently accessed or historical data to a separate collection or database optimized for long-term storage.

To apply the archive pattern, you move old or infrequently accessed data to separate collections to improve the performance of the main collection.

The following example includes message documents from two collections: one for active messages that are recent and frequently accessed, and another for long-term storage of old message documents:

```
// Active messages collection
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d7f5f"),
  conversationId: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6f"),
  sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  recipient: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  content: "Are we still meeting tomorrow?",
  timestamp: ISODate("2023-04-10T14:30:00Z"),
  read: true
}

// Archived messages collection (older than 90 days)
{
  _id: ObjectId("5f4e5b6d3c1e2a1b3c4d7f7f"),
  conversationId: ObjectId("5f4e5b6d3c1e2a1b3c4d7f6f"),
  sender: ObjectId("5f4e5b6d3c1e2a1b3c4d5e7a"),
  recipient: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),
  content: "Hey, how's it going?",
  timestamp: ISODate("2023-01-05T10:15:00Z"),
  read: true,
  archivedAt: ISODate("2023-04-05T00:00:00Z")
}
```

Let’s go over this example to understand how the archive pattern has been applied here:

- **Active messages collection:** The first document is part of the active collection, which stores recent data that is frequently accessed or updated, such as messages exchanged between users. This document represents a message sent on 2023-04-10 and remains in the active collection because it is considered part of ongoing interactions.
- **Archived messages collection:** The second document is part of an archived messages collection, which stores older messages that are no longer part of active interactions. This document represents a message sent on 2023-01-05, which was archived on 2023-04-05 after exceeding a 90-day threshold. The archivedAt field captures the timestamp when this message was moved to the archive.

By separating data into active and archived collections, this pattern keeps your active collections lean while preserving historical data. You can archive data into another collection in the same cluster, another cluster, or the Atlas online archive.

## Real-world application: The Socialite app

In real-world applications, achieving optimal performance often requires more than simply adopting a single schema design pattern; it involves combining multiple patterns. MongoDB’s flexibility allows developers to do just that! This section explores how combining schema design patterns can enhance performance, illustrated through practical examples of scenarios from the Socialite app.

### Scenario 1: User profile and activity feed

In the following scenario, the Socialite app needs to efficiently display a user’s profile alongside their recent activity feed. The traditional approach would be to store user data, posts, comments, likes, and follows in separate collections, and then perform complex joins across these collections every time a profile page is viewed. This method can be slow and resource-intensive, especially as the volume of data grows. However, with MongoDB, we can optimize this process by using a combination of schema design patterns: the computed pattern and the subset pattern.

The following example demonstrates this approach. It shows a single document from the users collection:

```
// User document
{
  _id: ObjectId("5f3d7eb8c242d20df83d3a8c"),
  username: "jane_smith",
  name: "Jane Smith",
  bio: "Photographer and travel enthusiast",
  profileImage: "https://socialite.com/images/profiles/jane_smith.jpg",
  followerCount: 1245,  // Computed pattern
  followingCount: 362,  // Computed pattern

  // Subset pattern - most recent activities
  recentActivities: [
    {
      type: "post",
      timestamp: ISODate("2023-04-07T10:22:15Z"),
      content: "Just posted photos from my Japan trip!",
      imageUrl: "https://socialite.com/images/posts/japan123.jpg",
      likeCount: 48
    },
