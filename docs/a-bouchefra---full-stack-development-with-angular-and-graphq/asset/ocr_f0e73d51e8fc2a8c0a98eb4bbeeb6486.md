Then, add a division for submitting comments, as follows:

```
```html
<div class="comment-box" *ngIf="commentsShown">
  <textarea #commentInput
    placeholder="Leave a comment.."
    matInput
    (keydown.enter)="createComment($event)">
  </textarea>
</div>
```
```

We conditionally render this HTML division, which contains a <textarea> element, referenced with a #commentInput reference, for entering comments. We bind the keydown.enter event of the <textarea> element to the createComment() method to allow users to create comments by pressing the Enter key on the keyboard.

Finally, add a button for loading more comments, as follows:

```
```html
<button mat-button class="load-comments-btn"
  (click)="loadComments()"
  *ngIf="commentsShown && post.commentsCount >
    commentsPerPost">
  Load more comments
</button>
```
```

This displays a Material button with the click event bound to the loadComments() method for loading more comments.

14. Open the post.component.css file and start by adding the following styles to it:

```
```css
* {
  box-sizing: border-box;
}
mat-card {
  margin: 0.4rem;
}
.post-wrapper {
  width: 100%;
}
.post-card-image {
  margin:0px;
}
```
```