Since the post component is dumb, its role consists of emitting events to the parent component that delegates tasks for creating comments and likes to the appropriate services.

Let's implement the handlers of both these events in the base component and bind them on the <app-post> element. Proceed as follows:

+   1. Open the core/components/base.component.ts file and start by importing both CommentsService and LikesService and the results' types, and then inject the services, as follows:

```
import { CommentsService }
  from './services/comment/comments.service';
import { LikesService }
  from '../services/like/likes.service';
```

Then, define the services' instances, as follows:

```
public commentsService: CommentsService;
public likesService: LikesService;
```

Finally, inject them inside the constructor, like this:

```
this.commentsService = injector.get(CommentsService);
this.likesService = injector.get(LikesService);
```

We use the get() method of Angular's injector instance (which is injected via the constructor) to inject CommentsService and LikesService, and then we assign the instances to the commentsService and likesService properties.

You also need to add the following highlighted types to the existing import declaration:

```
import {
  CommentEvent,
  LikeEvent,
  PostEvent,
  RemovePostEvent
} from 'src/app/shared';
```

+   2. Add the following method for handling the comment event:

```
onComment(e: CommentEvent): void {
  this.commentsService
    .createComment(e.comment, e.postId)
```