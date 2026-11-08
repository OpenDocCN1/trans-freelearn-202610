```
.pipe(take(1))
.subscribe({
    error: (err) => this.handleErrors(err)
});
}
```

When the comment event is emitted from the child post component, this method will be used to handle the event. It takes as an argument an object of the custom CommentEvent type.

Inside the method, we call the createComment() method of CommentService, and we pass the comment and post ID arguments that we get from the event object emitted from the child component. We subscribe to the returned Observable, and then we pass an observer object with an error handler that simply invokes and passes the error object to the handleErrors() method.

We use the take() operator to get the emitted value and unsubscribe from the returned Observable.

+   3. Implement an onLike() method for handling the custom like event, like this:

```
onLike(e: LikeEvent): void {
  if (e.post.likedByAuthUser) {
    this.likesService
      .removeLike(e.post.id)
      .pipe(take(1))
      .subscribe({
        error: (err) => this.handleErrors(err)
      });
  } else {
    this.likesService
      .likePost(e.post.id)
      .pipe(take(1))
      .subscribe({
        error: (err) => this.handleErrors(err)
      });
  }
}
```