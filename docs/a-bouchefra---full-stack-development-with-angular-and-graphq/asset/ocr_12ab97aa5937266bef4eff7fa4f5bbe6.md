```
```
    commentRepository: Repository<Comment>;
    likeRepository: Repository<Like>;
    notificationRepository: Repository<Notification>;
  };
};
```
```

This type will be used to type the context object that will be passed to the resolvers. At this point, we only pass an orm object containing the TypeORM repositories for working with the database.

+   4. Add the following code in the startApolloServer() method right underneath the app.use(cors()) ; line:

```
```
    const userRepository: Repository<User> =
        getRepository(User);
    const postRepository: Repository<Post> =
        getRepository(Post);
    const commentRepository: Repository<Comment> =
        getRepository(Comment);
    const likeRepository: Repository<Like> =
        getRepository(Like);
    const notificationRepository:
        Repository<Notification> =
        getRepository(Notification);
```
```

Next, define the context object of the Context type and add the TypeORM repositories to it, as follows:

```
```
    const context: Context = {
        orm: {
            userRepository: userRepository,
            postRepository: postRepository,
            commentRepository: commentRepository,
            likeRepository: likeRepository,
            notificationRepository: notificationRepository
        }
    };
```
```