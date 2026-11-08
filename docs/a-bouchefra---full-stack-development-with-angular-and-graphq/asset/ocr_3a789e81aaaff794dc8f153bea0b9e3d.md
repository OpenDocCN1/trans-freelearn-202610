This is all good magic for us! Except for one side effect—the post element will get initialized, and properties such as commentsShown will have their initial false value, which will hide the comment box (and the comments' feed later) and display the latest comment.

Here is a screenshot of a post after liking and commenting on it:

![](bbox=[0.12, 0.22, 0.85, 0.48])

Figure 10.5 – A post after adding a comment and a like

This is not the desired behavior since we don't want the component to reset its internal state every time a user comments on or likes a post. Instead, the user should be presented with the comment box and the comments' feed (that will be implemented later) until they choose to hide them. We'll see how to implement these in the next chapter.

# Summary

We've shown how to enhance our authentication system to deal with token expiration and how to utilize Apollo Client to send queries and mutations to our previously developed backend application programming interface (API). We've continued working on the profile component, adding code for getting paginated posts and comments, as well as making comments and likes.

In the following chapter, we'll finish up building the profile component before learning how to add realtime support to our application so that we can retrieve and display new data from the server without having to constantly refresh the app.