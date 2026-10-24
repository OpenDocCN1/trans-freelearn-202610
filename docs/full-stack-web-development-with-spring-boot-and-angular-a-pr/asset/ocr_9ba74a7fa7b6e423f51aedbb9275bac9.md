Finally, we must get the anti-heroes list feature. We can do this by dispatching the GET_ANTI_HERO_LIST action in the list component in the ngOnInit() hook. This will call the effect that we created earlier, which invokes the endpoint for getting the list of anti-heroes:

```
ngOnInit(): void {
  this.store.dispatch({type: AntiHeroActions.GET_ANTI_HERO_LIST});
  this.assignAntiHeroes();
}
```

With that, we have successfully created selectors for the components to retrieve data from the state. In the next section, we will discuss the available configurations we can implement for the store.

# Configuring the store

In the previous sections, we created all of the building blocks of NgRx that complete a fully functional store for the application. In this section, we will learn how to configure the NgRx store using runtime checks.

# Runtime checks

Runtime checks are used to configure the NgRx store to allow developers to follow the NgRx and Redux core concepts and best practices. This is very useful, especially for developers new to NgRx; they display errors regarding development based on the activated runtime checks.

@ngrx/store provides six built-in runtime checks:

+   - `strictStateImmutability`: Checks if the state isn't mutated (default: On)
+   - `strictActionImmutability`: Checks if the actions aren't mutated (default: On)
+   - `strictStateSerializability`: Checks if the state is serializable (default: On)
+   - `strictActionSerializability`: Checks if the actions are serializable (default: Off)
+   - `strictActionWithinNgZone`: Checks if actions are dispatched within NgZone (default: Off)
+   - `strictActionTypeUniqueness`: Checks if the registered action types are unique (default: Off)

To change the default configuration of the runtime checks, we will use the runtimeChecks property on the root store's config object. The value of each runtime check can be assigned with true to activate the check or false to deactivate the check:

```
@NgModule({
  imports: [
    StoreModule.forRoot(reducers, {
```