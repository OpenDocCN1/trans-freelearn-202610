```
notificationRepository.findOne(args.id,
    {relations:['user']});
if(!notification){
    throw new ApolloError("Notification not found",
        "NOTIFICATION_NOT_FOUND");
}
const result: DeleteResult = await
    notificationRepository.delete(args.id);
if(result.affected && result.affected <= 0) {
    throw new ApolloError("Notification not deleted",
        "NOTIFICATION_NOT_DELETED");
}
return args.id;
}
```

It's worth noting that we didn't include a mutation to create notifications. This is due to the fact that they must be created whenever people comment or like content. As a result, we can simply create them whenever new comment or like entities are added.

We can do this by using the AfterInsert decorator on some of the Comment and Like entities' methods, which will be called after the entities are inserted into the database:

+   1. Open the server/src/entity/Comment.ts file and add the following imports:
+   2. Add the createNotification() method to the class and annotate it with the AfterInsert decorator, as shown:

```
import { AfterInsert, getRepository } from 'typeorm';
import { Notification } from './Notification';
```

```
@AfterInsert()
async createNotification() {
    if (this.post && this.post.id) {
        const notificationRepository =
            getRepository(Notification);
        const notification =
            notificationRepository.create();
```