
```
margin: 0.4rem;
}
.profile-cover-card {
    margin: 0px;
    padding: 0px;
}
```

These styles are used to set box-sizing to border-box, which instructs the browser to take any border and padding into account when calculating the width and height values we give to our elements.

Next, we set the margin for the Material Card elements, and we set the margin and padding for the card containing the profile cover image and user's photo to 0 pixels each.

- 2. Style the element(s) with the .profile-cover class (the <div> element containing the cover image) as follows:

```
.profile-cover {
    height: 10rem;
    width: 100%;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    background-color:rgb(97, 76, 76);
    position: absolute;
}
```

To take up the entire width of the page, we set the width to 100%, and then we set the height to 10rem. The background's properties are then set to center for the position and no-repeat for the repeat attribute. The most important thing is that we set the background-size property to cover and use the absolute positioning.

Check out https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Backgrounds_and_Borders/Resizing_background_images.

3. Add the following styles of the user's image:

```css
.profile-image {
  width: 9rem;
  height: 9rem;
  clip-path: circle(60px at center);
  background-color: gray;
  border-width: 1px;
  margin-top: 5rem;
}
```

In this case, we set the width and height to the same value of 9rem, clip the image to a circle using the clip-path property, and set a default background for the user avatar that will be displayed if the user does not upload a photo.

Check out https://developer.mozilla.org/en-US/docs/Web/CSS/clip-path.

4. Next, let's style the element with CSS Grid layout:

```css
.profile-grid {
  display: grid;
  grid-template-columns: 1fr 2.5fr 1.5fr;
}
.profile-bio-btn {
  margin: 0.4rem;
  width: 100%;
}
.profile-bio-box {
  width: 100%;
  height: 5rem;
}
.bio-box-wrapper > textarea {
  padding: 10px;
  background-color: #f0f2f5;
  border-radius: 9px;
}
```

The profile-grid class includes all of the information below the cover image and user's photo, as well as the elements containing the full name, bio, date, post counts, and posts.

We switch the display to grid mode and divide the available horizontal space into three columns with ratios of 1, 2.5, and 1.5 using the fr unit (the fractional unit). Check out https://www.techiediaries.com/css-grid-layout-tutorial/ for a detailed explanation.

5. Style the element that will be displayed if no user matching the corresponding profile user ID is found:

```
.user-not-found {
    text-align: center;
    font-size: 5vw;
    margin: 100px;
}
```

6. For small screens with a maximum width of 600px, add the following styles:

```
@media (max-width: 600px) {
    .profile-grid {
        display: grid;
        grid-template-columns: 1fr;
    }
}
```

If the screen width is less than 600px, we only use one column for our profile grid element, which takes up all of the available horizontal space.

After you've added these styles, you'll have a profile that looks as in the following screenshot after creating an account and adding both the cover and photo images:

![](img/c798729e4c42765fcb1c98950164d0ba_298_0.png)

Figure 9.1 – The top section of the profile component

The following is a screenshot of the user's bio section after clicking the Edit.. button:

![](img/c798729e4c42765fcb1c98950164d0ba_299_0.png)

Figure 9.2 – User's bio section

The following is a screenshot of the user's bio section after adding a bio:

![](img/c798729e4c42765fcb1c98950164d0ba_299_1.png)

Figure 9.3 – User's information section

Now that we've finished implementing the user's information section of our profile component, let's move on to the user's posts section.

## Implementing the create post component

In this section, we'll implement the create post component, which will be used to create posts from the profile page (and also from the home feed). We'll put the logic for creating posts in its own reusable component because it will be required on multiple pages.

This component is known as a **presentational** or **dumb** component. It receives input and emits custom output events to the parent component. This type of component has no idea how to fetch data (no injected services), so its only responsibility is to receive data, render it, and emit some events for the parent component(s) to handle. These parent components in our case are the profile and feed's posts components, which are referred to as the **container** or **smart** components.

When this component's post event fires, the onPost() method of the base component that we defined earlier will be called. This custom event will be triggered when the user clicks on a button for creating posts, which we will implement in the following steps:

- 1. Begin by generating a component by running the following command from your client/ folder:
- 2. Export the create-post component from the shared/shared.module.ts file as follows:
- 3. Start by adding the following imports to the create-post.component.ts file in the core/components/create-post/ folder:

```
ng g c core/components/create-post --module=shared
```

```
@NgModule({
  declarations: [CreatePostComponent],
  imports: [
    CommonModule, ReactiveFormsModule, ...matModules
  ],
  exports: [ReactiveFormsModule, ...matModules,
    CreatePostComponent]
})
```

```
import {
  Component,
  ElementRef,
  EventEmitter,
  Input,
}
OnInit,
Output,
ViewChild
} from '@angular/core';
import { User }
from '@ngsocial/graphql/types';
import { PostEvent }
from 'src/app/shared';
```

We import the Angular core module's APIs, the User type, and PostEvent we defined previously.

4. Define the component's public and private properties as follows:

```
@Input() authUser!: Partial<User>;
@Input() loading: boolean = false;
@Output() post: EventEmitter<PostEvent> = new
EventEmitter();
public imageFile: File | null = null;
@ViewChild('postText') postText!: ElementRef;
```

We add two custom input properties for receiving the authenticated user and loading state, via the parent component, as well as a custom event that will be fired when users click the button to create a post.

Using input properties and events, we ensure parent-child communication between components. For more information, see https://angular.io/guide/inputs-outputs#sending-data-to-a-child-component.

We decorate the class properties with the @Input decorator to create input properties, and we can create a custom event by creating an instance of EventEmitter and exposing it as a property decorated with @Output that parent components can bind to, to listen for the custom event and access the associated data via a $event object.

TypeScript will throw errors if we do not initialize all of the defined component properties with default values. This is because Angular 12+, by default, enables the strict mode's strictPropertyInitialization rule.

We also define the `imageFile` property, which contains the user's selected image if it exists, and the `postText` property, which we decorated with `ViewChild` to allow us to get the text value of the input element (referenced by the `#postText` template reference) in the associated template:

1. Define the following getter to obtain the first name of the authenticated user:

```
get userFirstName() {
    return this.authUser
        ?.fullName
        ?.split(' ')
        ?.shift();
}
```

2. Define the following method to obtain the user's selected image:

```
onFileSelected(event: Event): void {
    const files: FileList =
        (event.target as HTMLInputElement)
        .files!;
    if (files.length > 0) {
        this.imageFile = files[0];
    }
}
```

This method will be bound to the `change` event of the input field used to select an image from the user's computer. The image file information is extracted from the event and stored in the `imageFile` property.

3. Define the following method for handling the button's click event to create a post:

```
handlePostClick(): void {
    if (this.authUser!.id) {
        this.post.emit({
            text: this.postText?.nativeElement.value,
            image: this.imageFile as File
        });
    }
    if (this.postText.nativeElement) {
        this.postText.nativeElement.value = '';
    }
}
```
