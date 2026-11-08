This is the response I received:

![](bbox=[0.22, 0.12, 0.73, 0.46])

Figure 4.4 - The response of the post mutation

You can see that the author is the same as the authenticated user that we used when signing in using the signIn mutation. It was retrieved successfully from the token we passed via the authorization header. This gives us a sign that our implementation is working as expected.

Next, let's create some comments and likes on these posts:

```
```graphql
mutation CreateComments {
    c1: comment(comment: "Comment on post with id #2",
        postId: 2 ) {
        id comment author { id fullName } post { id }
    }
    c2: comment(comment: "Comment on post with id #3",
        postId: 3 ) {
        id comment author { id fullName } post { id }
    }
    c3: comment(comment: "Comment on post with id #4",
        postId: 4 ) {
```
```