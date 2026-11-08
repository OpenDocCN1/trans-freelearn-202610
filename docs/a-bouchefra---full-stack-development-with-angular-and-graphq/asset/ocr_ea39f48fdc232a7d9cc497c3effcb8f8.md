7. Finally, display the notifications count in the template using a Material badge. Open the src/app/core/components/header/header.component.html file and update the Notifications button, as follows:

```
<button *ngIf="isLoggedIn" mat-icon-button matTooltip="Notifications">
  <mat-icon aria-hidden="false" aria-label="Notifications" [matBadge]="
    "notificationsCount" matBadgeColor="warn">
    notifications
  </mat-icon>
</button>
```

Here, we added the matBadge attribute to the <mat-icon> element and bound it to the notificationsCount property of the component.

That's it – you should now be able to receive and display the notifications of new comments and likes in your application. Check out the commit at https://git.io/JDuoK.

You can always extend this implementation to display more information about the notifications the users receive.

# Summary

In this final chapter, we learned how to configure Apollo Client for GraphQL subscriptions and then added code to display notifications in the application's header using a badge.

Throughout this book, we have used cutting-edge technologies such as Angular and GraphQL to develop a full stack application with a monorepo architecture using Lerna. We used Apollo on both the client and server to send and respond to GraphQL queries.

After setting up the development environment and installing Node.js, we built a server with GraphQL support to implement the backend using Express.js and Apollo Server. We also used Apollo Studio to communicate with our GraphQL API before we started developing a frontend application to consume the API and present a user interface to users to communicate with the backend.

After that, we explained how to install Express.js and configure it with TypeScript and GraphQL. We used mocking to provide a working GraphQL server with Apollo Server before implementing the resolvers that are responsible for fetching and adding data.