```
selectAuthState,
    (state: AuthState) => state.error
)
```

In the preceding code example, we have created a selector for our error field; we will need this selector to display the error message in our component for the user.

# Syncing in local storage

The next feature we will implement is the syncing of our state in local storage. We can achieve this by using localStorage.setItem() in our application. However, using this will not be maintainable, and the setting of values in the storage will be in different places.

To have a better implementation, we will use the ngrx-store-localstorage library. To install the library, we will execute the following command:

```
npm install ngrx-store-localstorage --save
```

After successfully installing the library, we should determine the states we want to sync with our local storage. In our case, we want the token field in our auth state to be synced. To achieve this, we make the following code changes in auth.module.ts:

```
import { localStorageSync } from 'ngrx-store-localstorage';
export function localStorageSyncReducer(reducer: ActionReducer<any>): ActionReducer<any> {
  return localStorageSync({keys: ['token']})(reducer);
}
const metaReducers: Array<MetaReducer<any, any>> = [localStorageSyncReducer];

@NgModule({
  declarations: [
    ... declared components
  ],
  imports: [
    ... other imported modules
    StoreModule.forFeature('authState', authReducer, {metaReducers}),
    EffectsModule.forFeature([AuthEffects]),
  ]
})
```