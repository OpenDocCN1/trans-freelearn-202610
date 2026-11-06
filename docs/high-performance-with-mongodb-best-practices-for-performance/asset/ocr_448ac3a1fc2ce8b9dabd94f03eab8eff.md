```
_id: ObjectId("5f4e5b6d3c1e2a1b3c4d5e6f"),  // Reference to complete user
user
  username: "jane_smith",    // Duplicated for display
  name: "Jane Smith",        // Duplicated for display
  avatar: "https://socialite.com/images/avatars/jane_smith.jpg" // Duplicated for display
  }
}
```

In this example, the posts document utilizes the extended reference pattern by including an author sub-document:

+   - **Embedded reference:** The author sub-document contains an _id field, which is a reference to the complete Author/User document stored elsewhere in a users collection. This allows linking the post back to its author.
+   - **Duplicated key fields for display:** Instead of just storing the _id value of the user, additional author-related details such as username, name, and avatar are included directly within the posts document. These fields are copied from the referenced users document to enhance performance and allow displaying author details without needing to perform additional queries or a lookup operation.
+   - **Optimized read operations:** By embedding username, name, and avatar, the posts document ensures that common queries, such as showing posts content along with the author's basic information, can be handled seamlessly without additional database calls. This reduces latency and improves efficiency for applications that frequently display posts and their authors.

Using the extended reference pattern here strikes a balance between normalization and denormalization. While _id provides a definitive link to the original Author/User document for updates or deeper data retrieval, the duplicated fields, such as username and avatar, ensure fast rendering of posts with essential author information. This pattern is particularly useful in read-heavy applications such as social media platforms.

# Subset pattern

The subset pattern is used in MongoDB to store frequently accessed data separately from infrequently accessed data, optimizing reads for common queries while keeping more detailed or less frequently needed information available in a related document.