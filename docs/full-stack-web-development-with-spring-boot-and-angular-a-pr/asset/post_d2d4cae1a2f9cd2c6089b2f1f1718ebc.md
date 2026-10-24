
Now that we have identified the list of actions we will create, in the `anti-heroes` folder, we will create a `state/anti-hero.actions.ts` file where we will place all of our actions.

Let’s place the following code in the `anti-hero.actions.ts` file:

```
```typescript
import { createAction, props } from '@ngrx/store';
import { AntiHero } from '../models/anti-hero.interface';
export enum AntiHeroActions {
  GET_ANTI_HERO_LIST = '[Anti-Hero] Get Anti-Hero list',
  SET_ANTI_HERO_LIST = '[Anti-Hero] Set Anti-Hero list',
}
export const getAntiHeroList = createAction(
  AntiHeroActions.GET_ANTI_HERO_LIST,
);
export const setAntiHeroList = createAction(
  AntiHeroActions.SET_ANTI_HERO_LIST,
  props<{ antiHeroes: ReadonlyArray<AntiHero> }>(),
);
```
```

In the preceding code example, we have created two actions for getting and setting the anti-hero list. The first action, `getAntiHeroList`, has a single parameter, which is the type. This does not need any additional properties as this will only call the API to retrieve the list of anti-heroes through the use of effects.

On the other hand, the second action, `setAntiHeroList`, has two parameters: the type and an additional property called `antiHero`. This will set the value of the store with the retrieved list of anti-heroes through the use of reducers.

It is also excellent practice to make our action definitions enums in our code as this will help us avoid typographical errors when using the definition in other parts of the application.

With that, we have successfully created the required action for our anti-heroes list feature. Now, we will discuss how to write effects that will be used to call the API and retrieve the anti-hero list.

## Writing an effect

We successfully created our actions in the previous section. Now, we will make the effects that will be responsible for calling our external API.

Effects are not included in the `@ngrx/store` library; we will install a separate library that will allow us to use the functions for effects.

To install effects in our application, we must execute the following command:

```
ng add @ngrx/effects
```

The preceding command will perform the following steps:

- 1. Update the package.json file with the @ngrx/effects dependency.
- 2. Run npm install to install the added dependency.
- 3. Add EffectsModule.forRoot() to the imports array of your app.module.ts file.

Some flags are available that allow us to install @ngrx/effects with customizations; the following is the list of flags we can use:

- - --path: Specifies the path to the module where you want to import EffectsModule.
- - --project: The name of the project defined in angular.json.
- - --skipTests: This creates a test file when it is set to false.
- - --module: The name of the file containing the module where you want to import EffectsModule.
- - --minimal: This provides the minimal setup for the root effects if set to true. It imports EffectsModule.forRoot() in the module with an empty object.
- - --group: Group the effects files within the effects folder.

After successfully adding the dependency of the effect to our application, we can create our effects. Under the anti-heroes/state folder, we must create a new file named anti-hero.effects.ts. The first thing we need to do is create a class that has an @Injectable annotation:

```
@Injectable()
export class AntiHeroEffects {
  constructor(
    private actions$: Actions,
    private antiHeroService: AntiHeroService,
    private router: Router
  ) {}
}
```

In the preceding code example, we can see that effects are also service classes and can be injected by other services; we have injected the following services into AntiHeroEffects:

- - Actions: A service from @ngrx/effects that returns an observable that we can assign a type to. This will act as the identifier when an action is dispatched.
- - **AntiHeroService**: Our created service that contains the external APIs for our anti-heroes, found under `anti-hero/services/anti-hero.service.ts`.
- - **Router**: Used for redirection once an API call is made.

After creating our `AntiHeroEffect` class and injecting our services, we can begin making our effects. The first thing we need to think of is what kind of effect we need to get the anti-heroes since we have the `GET_ANTI_HERO_LIST` and `SET_ANTI_HERO_LIST` actions.

We should create an effect that has a type of `GET_ANTI_HERO_LIST` and can call the `getAntiHeroes()` function from `AntiHeroService`.

To create this API, we can write the following code:

```
```typescript
import { Actions, createEffect, ofType } from '@ngrx/effects';
getAntiHeroes$ = createEffect(() => {
    return this.actions$.pipe(
        ofType(AntiHeroActions.GET_ANTI_HERO_LIST),
        mergeMap(() => this.antiHeroService.getAntiHeroes()
        )
    }, {dispatch: true}
);
```
```

In the preceding code example, we used the `createEffect()` function to create our effects; this returns an action that has two parameters:

- - `ofType(AntiHeroActions.GET_ANTI_HERO_LIST)`: The first parameter uses the `ofType` operator, which defines the action type of the effect. This means that if the `GET_ANTI_HERO_LIST` action is dispatched, this effect will be called.
- - `mergeMap(() => this.antiHeroService.getAntiHeroes())`: The second parameter uses the `mergeMap` operator, which will allow us to invoke the `getAntiHeroes()` function to call the endpoint.

With that, we have our effect for the `GET_ANTI_HERO_LIST` actions, but this is not complete yet. After getting the list of anti-heroes, we want to dispatch another action that sets the anti-heroes list in our state. To implement this, we can use the following code:

```
```typescript
mergeMap(() => this.antiHeroService.getAntiHeroes()
    .pipe(
        map(antiHeroes => ({ type: AntiHeroActions.SET_ANTI_HERO_LIST, antiHeroes })),
        catchError(() => EMPTY)
    ));
```
```

In the preceding code, we have added a pipe to our mergeMap operator; this calls a map operator that returns ({ type: AntiHeroActions.SET_ANTI_HERO_LIST, antiHeroes }). This will dispatch another action that has a type of SET_ANTI_HERO_LIST and has additional antiHeroes objects coming from the list of anti-heroes retrieved from the API.

Our effects for getting the list of anti-heroes feature is complete. The last step is to add AntiHeroEffects to our effects module. As we may recall, our anti-heroes module is lazy loaded, which means that we will not add AntiHeroEffects to EffectsModule.forRoot([]) located in the app.module.ts file; otherwise, we would need to add EffectsModule.forFeature([AntiHeroEffects]) in the imports of the anti-hero.module.ts file. This means that this effects class is only used under this module.

With that, we have successfully configured and created our effects for the anti-hero list feature. In the next section, we will write the reducers that will modify our state.

## Writing a reducer

NgRx states are immutable objects; we cannot modify their values through direct assignment, and the only way we can change their states is through reducers.

Reducers have different parts that we should implement, as follows:

- - The interface or type that defines the properties of the state
- - The arguments, which consist of the initial state and the current action
- - The list of functions that handle that state changes based on the dispatched actions

We will create these reducer parts under the anti-heroes/state/anti-hero reducers.ts file.

### The state interface

The state interface defines the shape of the state; this contains the properties or the slices of the state. In our application, we need a property that will hold the list of anti-heroes.

To implement the interface, we can use the following code:

```
export interface AntiHeroState {
  antiHeroes: ReadonlyArray<AntiHero>;
}
```

### The initial state

The next part we need to implement is the initial state; this defines what the initial values of the state slices are. In our anti-hero state, we will set the antiHeroes slice to an empty array.
