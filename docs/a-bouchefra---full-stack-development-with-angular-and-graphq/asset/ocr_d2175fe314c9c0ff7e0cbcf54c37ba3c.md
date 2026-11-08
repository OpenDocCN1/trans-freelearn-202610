+   - Creating the profile UI
+   - Implementing the `create-post` component

# Technical requirements

To complete this chapter's steps, you must first finish the previous chapter of this book.

You should also be acquainted with the following technologies:

+   - JavaScript/TypeScript
+   - HTML
+   - CSS

This chapter's source code can be found at https://github.com/PacktPublishing/Full-Stack-App-Development-with-Angular-and-GraphQL/tree/main/Chapter07.

# Image uploading with Angular and Apollo

Uploading images is a required feature of any social network; in our case, we need images not only for posts, but also for the user's photo and profile cover.

We've added three mutations to the backend: `uploadFile`, `setUserPhoto`, and `setUserCover`. In this section, we'll write the code for sending requests to call these GraphQL mutations to create posts with or without an image, as well as setting the user's profile photo and cover.

We manually created the GraphQL documents in the previous section by wrapping the queries and mutations with the `gql` tag, which returns the documents (objects of the `DocumentNode` type) that we pass to Apollo Client methods to fetch or mutate data.

Then, for communicating with our GraphQL API, we created types for results/responses and implemented the Apollo services, which extend the generic `Query` or `Mutation` classes.

This works, but as our code base grows, it is unlikely to scale because manually writing and maintaining types requires extra work to ensure the types of the results (and variables) match the selection sets (and GraphQL arguments) we are working with. This increases the likelihood of errors such as typos.