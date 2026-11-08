```
notification.user = await getRepository(
    User).createQueryBuilder("user")
    .innerJoinAndSelect("user.posts", "post")
    .where("post.id = :id", { id: this.post.id })
    .getOne() as User;
    notification.postId = this.post.id;
    notification.text = '${this.author.fullName}
    commented on your post';
    await notificationRepository.save(notification);
  }
}
```

Using the methods of the notification repository, we create the necessary notification within the method.

Then we must repeat the process on the Like entity:

1. Add the following imports to the server/src/entity/Like.ts file:

```
import { AfterInsert, getRepository } from 'typeorm';
import { Notification } from './Notification';
```

2. Add an @AfterInsert-decorated method where you must implement the code to provide a suitable notification informing users that someone liked their post:

```
@AfterInsert()
async createNotification() {
  if (this.post && this.post.id) {
    const notificationRepository =
      getRepository(Notification);
    const notification =
      notificationRepository.create();
    notification.user = await getRepository(
      User).createQueryBuilder("user")
      .innerJoinAndSelect("user.posts", "post")
      .where("post.id = :id", { id: this.post?.id })
      .getOne() as User;
    notification.postId = this.post?.id;
    notification.text = '${this.user.fullName} liked
      your post';
```