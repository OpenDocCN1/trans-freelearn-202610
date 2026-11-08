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