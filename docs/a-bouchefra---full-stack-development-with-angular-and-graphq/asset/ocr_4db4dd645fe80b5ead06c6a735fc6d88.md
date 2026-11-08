```
@Output()
remove = new EventEmitter<RemovePostEvent>();
@Output()
listLikes = new EventEmitter<DisplayLikesEvent>();
```

We define output properties and decorate them with the @Output decorator; then, we assign an instance of EventEmitter to each one and set the type parameter of the generic EventEmitter class to the appropriate custom event type that we defined earlier. The type passed corresponds to the type of object that will be sent to the parent components.

7. Define the properties, as follows:

```
@ViewChild('commentInput')
commentInput!: ElementRef;
commentsShown: boolean = false;
```

We define a commentInput property of the type ElementRef and decorate it with the @ViewChild decorator so that we can access the element with the commentInput reference in the associated template. Then, we define a commentsShow property for controlling whether comments are displayed.

8. Add the following getter, which simply returns the number of likes concatenated with a Likes string:

```
get latestLike(): string {
  return '${this.post?.likesCount ?? 0} Likes';
}
```

Next, define the stub methods, as follows:

```
displayLikes(): void {}
displayComments(): void {}
loadComments(): void {}
sendLike(): void {}
removePost(): void {}
createComment(e: Event): void {}
```

The code for these methods will be added later, but for now, they are required in order to display the component without errors.