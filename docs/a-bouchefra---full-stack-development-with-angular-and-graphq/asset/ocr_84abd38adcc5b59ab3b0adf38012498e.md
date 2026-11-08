Now that we've formatted the post and latest comment dates using the "time ago" format, let's learn how to fetch and display comments.

# Displaying comments

Follow these steps to fetch and display comments:

+   1. In the core/components/base.component.ts file, import the Comment type by adding it to the existing import declaration:
+   2. In the same file, inside the base component, define the following property:
+   3. Add the onListComments() method for fetching comments to the base class:

```
import {
  User,
  Post,
  Comment
} from '@ngsocial/graphql/types';
```

Then, import the following highlighted types:

```
import {
  CommentEvent,
  LikeEvent,
  ListCommentsEvent, MoreCommentsEvent,
  PostEvent,
  RemoveCommentEvent,
  RemovePostEvent
} from 'src/app/shared';
```

```
public comments: Map<string,
  {
    result: Comment[],
    fetchMore: () => void
  }> = new Map();
```

This will be used to map the comments array and the fetchMore() function to the posts IDs, which will allow us to save and track the comments and pagination function of each post.

```
onListComments(e: ListCommentsEvent): void {}
```