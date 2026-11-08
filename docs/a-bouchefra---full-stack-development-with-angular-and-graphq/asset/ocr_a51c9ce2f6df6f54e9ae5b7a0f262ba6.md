The following topics will be covered:

+   - Improving the authentication system
+   - Adding queries and generating types and services
+   - Fetching paginated posts
+   - Implementing comments and likes services
+   - Implementing a presentational post component
+   - Displaying posts, comments, and likes

Here's a summary of what we'll do:

+   1. First, we'll write query and mutation strings that will be sent to the server.
+   2. We'll utilize the GraphQL generator to produce the types of the results and variables, as well as the GraphQL services (that extend the Query and Mutation classes of apollo-angular) to like and comment on posts.
+   3. We'll create and export Angular services, which include the logic for working with comments and likes and sending requests to the server.
+   4. We'll add posts-fetching-with-pagination logic to the previous chapter's post service. We'll provide two methods for retrieving a user's posts, as well as a feed of the most recent posts made by application members.
+   5. We'll implement a post component, a dumb component for displaying a single post, with comments and likes, as well as buttons for liking and commenting on the post.
+   6. Finally, the posts, comments, and likes will be displayed on the profile and home views.

# Technical requirements

To complete the steps in this chapter, you must first finish the previous chapter's steps and assignments and be familiar with the following technologies:

+   - JavaScript/TypeScript
+   - HyperText Markup Language (HTML)
+   - Cascading Style Sheets (CSS)

You can find the source code for this chapter at https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL/tree/main/Chapter08.