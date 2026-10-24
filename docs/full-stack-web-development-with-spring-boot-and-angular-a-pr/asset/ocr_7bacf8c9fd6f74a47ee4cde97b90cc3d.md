In the preceding code, we have added a pipe to our mergeMap operator; this calls a map operator that returns ({ type: AntiHeroActions.SET_ANTI_HERO_LIST, antiHeroes }). This will dispatch another action that has a type of SET_ANTI_HERO_LIST and has additional antiHeroes objects coming from the list of anti-heroes retrieved from the API.

Our effects for getting the list of anti-heroes feature is complete. The last step is to add AntiHeroEffects to our effects module. As we may recall, our anti-heroes module is lazy loaded, which means that we will not add AntiHeroEffects to EffectsModule.forRoot([]) located in the app.module.ts file; otherwise, we would need to add EffectsModule.forFeature([AntiHeroEffects]) in the imports of the anti-hero.module.ts file. This means that this effects class is only used under this module.

With that, we have successfully configured and created our effects for the anti-hero list feature. In the next section, we will write the reducers that will modify our state.

# Writing a reducer

NgRx states are immutable objects; we cannot modify their values through direct assignment, and the only way we can change their states is through reducers.

Reducers have different parts that we should implement, as follows:

+   - The interface or type that defines the properties of the state
+   - The arguments, which consist of the initial state and the current action
+   - The list of functions that handle that state changes based on the dispatched actions

We will create these reducer parts under the anti-heroes/state/anti-hero reducers.ts file.

# The state interface

The state interface defines the shape of the state; this contains the properties or the slices of the state. In our application, we need a property that will hold the list of anti-heroes.

To implement the interface, we can use the following code:

```
export interface AntiHeroState {
  antiHeroes: ReadonlyArray<AntiHero>;
}
```

# The initial state

The next part we need to implement is the initial state; this defines what the initial values of the state slices are. In our anti-hero state, we will set the antiHeroes slice to an empty array.