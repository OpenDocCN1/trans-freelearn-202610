Define the following members in the class's body:

```
```ts
public authService: AuthService;
public postService: PostService;
public posts: Post[] = [];
public authUser: Partial<User> | null = null;
public loading: boolean = false;
protected snackBar: MatSnackBar;
private componentDestroyed = new Subject();
```
```

We defined the properties that will be used in the parent components, such as the posts array, which will hold the fetched posts, the authUser object, which will hold the authenticated user, and the loading property, which will track the loading state of operations.

We also defined the componentDestroyed subject, which will be used to unsubscribe from the returned observables of various methods after the component has been destroyed.

Finally, we defined the authService, postService, and snackBar properties, which will be used to assign dependencies with the Injector service rather than injecting them via the constructor to simplify the child components.

The private attributes are only accessible from within the base component, whereas the protected attributes are accessible from both the base and child components. The public properties are accessible from within the components and the associated templates.

4. Next, instead of using the constructor's arguments, let's inject the dependencies inside the constructor:

```
```ts
constructor(public injector: Injector) {
  this.snackBar = injector.get(MatSnackBar);
  this.authService = injector.get(AuthService);
  this.postService = injector.get(PostService);
}
```
```

We inject the Injector service in the constructor and then use it to get the required dependencies. This eliminates the need for the child components to re-pass all of the parent dependencies into their constructor. For more information, check out https://devblogs.microsoft.com/premier-developer/angular-how-to-simplify-components-with-typescript-inheritance/.