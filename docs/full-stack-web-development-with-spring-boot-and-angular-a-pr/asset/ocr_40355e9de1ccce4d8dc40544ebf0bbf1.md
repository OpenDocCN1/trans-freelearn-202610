```
mergeMap(( (data: {type: string, payload: User}) =>
    this.authService.login(data.payload)
    .pipe(
        map(data => ({ type: AuthActions.SET_TOKEN,
            token: data.token })),
        tap(() =>
            this.router.navigate(["anti-heroes"])),
        catchError(async (data) => ({ type:
            AuthActions.AUTH_ERROR, error: data.error }))
    ))
}, {dispatch: true}
```

# Register Effect

```
createUser$ = createEffect(() => {
    return this.actions$.pipe(
        ofType(AuthActions.CREATE_USER),
        mergeMap(( (data: {type: string, payload: User}) =>
            this.authService.register(data.payload)
            .pipe(
                tap(() => this.router.navigate(["login"])),
                catchError(async (data) => ({ type:
                    AuthActions.AUTH_ERROR, error: data.error }))
            )
        )
    }, {dispatch: true}
);
```

In the preceding code, we have created effects for the login and register API. In the loginUser$ effect, once the login API is successful, it will dispatch the SET_TOKEN action and pass the generated JWT, and this will also redirect us to the anti-heroes page.

This is the same behavior we implemented in the previous section. On the other hand, the createUser$ effect, once the register API is successful, will redirect us to the login page again. This is a simple behavior, and you can customize what will happen next if the registration is successful.

We have also implemented the AUTH_ERROR action, which will be called when the login or register API fails.