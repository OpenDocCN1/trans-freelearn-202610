```
```ts
  post: Post;
};
```
```

Next, create a removelike.event.ts file and add the following code to it:

```
```ts
export type RemoveLikeEvent = {
  id: string;
};
```
```

Next, create a displaylikes.event.ts file and add the following code to it:

```
```ts
export type DisplayLikesEvent = {
  postId: string;
};
```
```

Also, export the following types from the shared/index.ts file:

```
```ts
export { LikeEvent }
  from './types/like.event';
export { RemoveLikeEvent }
  from './types/removelike.event';
export { DisplayLikesEvent }
  from './types/displaylikes.event';
```
```

4. Open the shared/components/post/post.component.ts file and start by adding these imports:

```
```ts
import {
  Input,
  Output,
  EventEmitter,
  ViewChild,
  ElementRef
} from '@angular/core';
```
```

Next, import the User and Post types, like this:

```
```ts
import {
  User,
  Post
} from '@ngsocial/graphql/types';
```
```