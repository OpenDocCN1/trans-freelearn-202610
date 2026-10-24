
## Application with side effects

The following is an application with side effects:

```typescript
export class BlogsPageComponent {
  blogs$: Observable<Blog[]> = this.store.select(state =>
    state.blog);

  constructor(private store: Store<{ blogs: Blog[] }>) {}

  ngOnInit() {
    this.store.dispatch({ type: '[Blog Page] Load Blog' });
  }
}
```

In the preceding code example, we can see that the code for our blog page component has been reduced, and, at the same time, its responsibility is also less complex. Now, the component’s responsibility is to dispatch an action that will allow effects to identify what service needs to be called to retrieve the data.

Let’s look at an example effect for the blog state:

```typescript
@Injectable()
export class BlogEffects {
  loadBlogs$ = createEffect(() => this.actions$.pipe(
    ofType('[Blog Page] Load Blog'),
    mergeMap(() => this.blogService
      .getAll().pipe(
        map(blogs => ({ type: '[Blogs API] Blogs Loaded Success', payload: blogs })),
        catchError(() => EMPTY)
      )
    )
  );
  constructor(private actions$: Actions,
              private blogService: BlogService) {}
}
```

In the preceding code example, we created a new effect named loadBlogs$. This effect is responsible for calling the getAll() method from the blog service to retrieve the list of blogs from the external endpoint. At the same time, it is also responsible for dispatching a new action that passes the retrieved blog list into the reducer to modify the store. We will discuss how to write effects in the next section of this chapter.

With that, we've seen all of the building blocks that make up state management. Let's look at a detailed graphical illustration of how data flows in state management:

![](img/7c3ddd2e5893c9b5a0220148ed886548_302_0.png)

Figure 12.3 – How data flows when using NgRx state management

In the preceding graphical illustration, we can see that our UI components have only one responsibility, and this is to dispatch actions. If the action needs to call an API, an effect will be called to invoke an API using a service, and after getting the response data, the effect will also dispatch an action to call a reducer to modify the store.

On the other hand, if the action that's sent from the component will alter the state, it will not need any side effects and call the reducer that matches the type of the action. All of the changes in the store will be detected by the selectors and emit the latest state to be used in the UI components.

With that, we have learned about the necessary concepts surrounding state management and how data flows together with the building blocks of state management. In the next section, we will learn how to set up and configure one of the most famous state management libraries in Angular: NgRx.

## Getting started and setting up NgRx

To use NgRx state management, we must install the @ngrx/store library; this will contain all of the functions that will allow us to configure the store and create reducers and actions.

To install the @ngrx/store library, we must execute the following command:

```
ng add @ngrx/store
```

The preceding command will perform the following steps:

- 1. Update package.json by adding @ngrx/store to the dependencies.
- 2. Run npm install to install the dependencies.
- 3. Update src/app/app.module.ts by adding StoreModule.forRoot(reducers, {}) to the imports array.

Before executing this command, make sure that the version of @ngrx/store is aligned with the version of your Angular; in our project, our Angular version is version 13.3.0, which means that we need to use version 13 of @ngrx/store.

Flags are also available that allow us to install @ngrx/store with customizations. The following is the list of flags we can use:

- - --path: Specifies the path to the module where you want to import StoreModule.
- - --project: The name of the project that is defined in angular.json.
- - --module: The name of the file containing the module where you want to import StoreModule.
- - --minimal: This provides the minimal setup for the root state management if set to true. It imports StoreModule.forRoot() in the module with an empty object.
- - --statePath: This is the path where the state will be created.
- - --stateInterface: The interface that defines the state.

## Adding NgRx to our Angular project

Now, let's add this to our Angular project. We only want to use the minimal setup as we will add the reducers and store step by step. After successfully executing the ng add @ngrx/store command, we will have the following changes in our project:

```
// app.module.ts
imports: [
  ... other modules
  StoreModule.forRoot({}, {}),
```

In the preceding code example, we can see that a new module called `StoreDevtoolsModule` has been added; this will allow DevTools to be used once we run our application locally.

The next step to using DevTools is to add the Redux extension to our browser. To add this extension, go to one of the following links for your respective browser:

- - **Google Chrome:** `https://chrome.google.com/webstore/detail/redux-devtools/lmhkpm...`
- - **Mozilla Firefox:** `https://addons.mozilla.org/en-US/firefox/addon/reduxdevtools/`

After adding this extension to your preferred browser, running your Angular project with the imported `StoreDevToolModule` module will be automatically detected by this extension. It will provide an interface for viewing the state:

![](img/7c3ddd2e5893c9b5a0220148ed886548_305_0.png)

Figure 12.4 – Interface of the Redux DevTools extension

The preceding screenshot shows that our Redux DevTools extension has been activated; once we open our Angular project in our browser tab, we will see more Redux DevTools in action as we write our code.

Now that we have configured our store and installed NgRx DevTools in our application, we are ready to write the building blocks for our state management.

## Writing an action

The first building block of state management is that we will write our actions. When writing actions, we have several rules we can follow so that we have good actions in our application:

- - **Upfront**: Writing actions should always come first before developing the features. This gives us an overview of what should be implemented in the application.
- - **Divide**: We should always categorize the actions based on the event source and the associated data.
- - **Many**: Writing more number actions is not an issue. It is more beneficial as more actions create a better overview of the flow of your application.
- - **Event-Driven**: Capture *events* as you separate the description of an event and how it's handled.
- - **Descriptive**: Always provide meaningful information using type metadata. This helps debug the state.

Let's look at an example action that will set the list of blogs in our state:

```
import { createAction, props } from '@ngrx/store';
export const setBlogList = createAction(
  '[Blog] Set Blog List',
  props<{ blogs: ReadonlyArray<Blog> }>(),
);
```

In the preceding code example, we used the `createAction()` function to create our action. The `createAction()` function also returns a function that returns an object of the action interface; in this case, it will return "[Blog] Set blog list" as the action type and the array of blogs as the additional properties.

To dispatch the action, we will use the `dispatch()` function and use `setBlogList` as a parameter:

```
ngOnInit() {
    store.dispatch(setBlogList({ blogs: this.blogs }));
}
```

## Implementing actions in the project

Our primary goal for our project is to create the entire cycle to get the list of anti-heroes in our API and place it in our store. The first step is to take the actions we need; let's look at the two actions that we should make for this feature:

- - `getAntiHeroList`: This action will retrieve the list of blogs from the external API provided by our Spring project.
- - `setAntiHeroList`: This action will place the retrieved list of blogs in our store.
