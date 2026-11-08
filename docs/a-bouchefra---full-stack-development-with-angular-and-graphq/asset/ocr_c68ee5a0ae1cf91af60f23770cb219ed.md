```
return qRef.valueChanges;
})
```

We assign the returned QueryRef object to the qRef constant, and then we assign an arrow function to the fetchMore property. This function simply wraps the qRef.fetchMore() method to set the offset variable to the posts.length value so that we only fetch posts that aren't already fetched.

Then, we return the valueChanges Observable, as in the previous step.

+   9. Add a button for fetching posts to the profile's template, as follows:

```
<button *ngIf="authUser && profileUser.postsCount && profileUser.postsCount > posts.length" mat-raised-button color="primary" (click)="fetchMore()">More</button>
```

We simply bind the click event of this button to the fetchMore() function available as a property on the component's class.

This button will be conditionally rendered only if the user is logged in and the fetched posts' length, posts.length, is less than the number of posts that belong to the profile's user retrieved from the profileUser.postsCount property. We use the *ngIf directive to conditionally render this button. We use the mat-raised-button directive to add Material styles and behavior to the button, and we give it a primary color.

> **Note**
> 
> The fetchMore() method is provided by Apollo for data pagination.
> Check out https://apollo-angular.com/docs/caching/interaction#incremental-loading-fetchmore for more information.

Let's move on to implementing code for adding comments and likes. When the user enters a comment and presses Enter, the createComment() method of the post component will be invoked.

We've already implemented this method, which emits the custom comment event to the parent profile component alongside required information, such as the comment itself and the post ID, and clears the comment box.

Similarly, when the user clicks on the Like button, displaying the black heart, the sendLike() method will be invoked and will emit the custom like event alongside the post that should be liked.