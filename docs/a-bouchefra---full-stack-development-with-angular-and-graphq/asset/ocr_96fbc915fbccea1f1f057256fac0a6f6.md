```
author { ...BasicUserFields }
  post { id }
}
}
```

This query document will be used to retrieve comments for a certain post. It accepts the post ID and the offset/limit arguments, for paginating comments, as input. In the selection set, we ask for the ID, comment text, and date, as well as sub-selections for author information (ID, full name, username, and image) and post (ID).

5. Add the following mutation to the same file to create a comment:

```
mutation commentPost(
  $comment: String!,
  $postId: ID!){
  comment(comment:$comment, postId:$postId){
    id post { id commentsCount }
  }
}
```

In this step, we define a GraphQL document that accepts the text of the comment and the post ID as input, both of which are required inputs. In the selection set, we select the ID of the new comment, the post's ID, and the commentsCount fields.

6. Add the following mutation to remove comments:

```
mutation removeComment($id: ID!){
  removeComment(id:$id){
    id post { id commentsCount }
  }
}
```

This mutation takes as an argument the ID of the comment to be removed.

7. Create a graphql.documents/like.graphql file and add the following query to it:

```
query getLikesByPostId(
  $postId: ID!,
  $offset: Int,
  $limit: Int){
  getLikesByPostId(
```