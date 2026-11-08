# Displaying posts, comments, and likes

After partially implementing our presentational post component, let's see how to use it in the parent user's profile component and use its input properties and custom events to communicate between the child and parent. Here are the steps we need to follow:

+   1. First, we need to include the post component in the profile component, so open the users/components/profile/profile.component.html file and add the following markup in the <div> element, with the posts-column class, below the <app-create-post> component:

```
<div *ngFor="let post of posts;">
  <app-post [post]="\"post\"" [authUser]="\"authUser\""
    (remove)="onRemovePost($event) ">
  </app-post>
</div>
```

We include a component in the template using the value of the selector attribute in the component's decorator, which is app-post in this case.

We can invoke this component without issues since it's exported from the shared module, which is, by itself, imported from the users module that contains the profile component.

We iterate over the posts array using the *ngFor directive and display each post using the post component. We pass in the post object via the post property of the component and the authenticated user using the authUser property. Then, we bind the custom remove event of the dumb component to the onRemovePost() method, which is implemented and inherited from the base component.

The component accepts other custom events, but we first need to define and implement handlers for these in the base class before binding them here. We add these handler methods to the base class to be able to reuse them from the post component of the feeds module.

+   2. Add the code for fetching the posts to the profile component's class. Open the users/components/profile/profile.component.ts file and start by importing the Post type by adding it to the existing import declaration, as follows:

```
import { Post, User }
from '@ngsocial/graphql/types';
```