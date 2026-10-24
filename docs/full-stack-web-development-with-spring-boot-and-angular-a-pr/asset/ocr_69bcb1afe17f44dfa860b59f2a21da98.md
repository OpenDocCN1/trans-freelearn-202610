# Creating the delete effect

The next step we need to do is to create the effect for the delete feature. In the `anti-hero/state/anti-hero.effect.ts` file, we will add the following code:

```
```typescript
// See full code on https://github.com/PacktPublishing/Spring-
Boot-and-Angular/tree/main/Chapter-13
removeAntiHero$ = createEffect(() => {
  return this.actions$.pipe(
    ofType(AntiHeroActions.REMOVE_ANTI_HERO_API),
    mergeMap((data: { payload: string }) =>
      this.antiHeroService.deleteAntiHero(data.payload)
        .pipe(
          map(() => ({ type:
            AntiHeroActions.REMOVE_ANTI_HERO_STATE,
            antiHeroId: data.payload })),
          catchError(() => EMPTY)
        )
    )
  )
}, { dispatch: true }
);
```
```

In the preceding code example, we can see that we have created a new effect for our delete action; this has a type of `REMOVE_ANTI_HERO_API`, which calls the `deleteAntiHero()` function in `AntiHeroService` for the data deletion based on the passed ID, and once the API is successful.

The effect will dispatch another action, `REMOVE_ANTI_HERO_STATE`, which we created in the previous section, which removes the anti-hero from the store. This means that the data we delete from the database will also be deleted from our NgRx store.

# Modifying the dispatch

The last step for this feature is to modify the action being dispatched in the `list.component.ts` file. In the previous section, we call the `REMOVE_ANTI_HERO_STATE` action directly in our component; we will change this into `REMOVE_ANTI_HERO_API` as we should now call the effect, which will invoke the API and at the same time will call the `REMOVE_ANTI_HERO_STATE` action.