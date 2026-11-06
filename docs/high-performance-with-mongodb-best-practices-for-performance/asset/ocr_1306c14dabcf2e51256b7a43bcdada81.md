# Real-world application: The Socialite app

In real-world applications, achieving optimal performance often requires more than simply adopting a single schema design pattern; it involves combining multiple patterns. MongoDB’s flexibility allows developers to do just that! This section explores how combining schema design patterns can enhance performance, illustrated through practical examples of scenarios from the Socialite app.

# Scenario 1: User profile and activity feed

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
```