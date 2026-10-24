Let’s have a look at the following code example:

```
selectAntiHero(data: {antiHero: AntiHero, action: TableActions}) {
  switch(data.action) {
    case TableActions.Delete: {
      this.store.dispatch({type:
        AntiHeroActions.REMOVE_ANTI_HERO_API,
        payload: data.antiHero.id});
      return;
    }
    default: ""
  }
}
```

In the preceding code example, we are now dispatching the effect in our list component. This will call the API first before updating our store in the application; the changes in our store and database are synced.

In the next section, we will implement the addition of data with side effects to our application.

# Adding an item with side effects using NgRx

In this section, we will implement the add functionality with side effects in NgRx. The steps are similar to how we implemented the delete feature. We will create the building blocks step by step and create the dispatch logic in our component.

# Creating the actions

The first step we need to do is create the required action types and actions for our add feature. To implement the actions, we can think of how we created the actions for the delete feature.

The concept is the same. There are two action types that we need to create, and these are ADD_ANTI_HERO_API and ADD_ANTI_HERO_STATE. The first type will be used by the effect that will call the API, and the second type will be used by the reducer that will modify the state by adding the newly created data.

After creating the two action types, we also need to create an action using the createAction() function for the ADD_ANTI_HERO_STATE type. The effect will dispatch this once the API has been successfully called.