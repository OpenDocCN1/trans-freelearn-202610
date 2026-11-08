10. Finally, flush the fake response to make the Observable emit, as follows:

```
op.flush({ data: fakeRegisterResponse });
```

You should have two passed specs after saving and returning to your Karma report, as indicated in the following screenshot:

![](bbox=[0.37, 0.22, 0.63, 0.36])

Figure 8.3 – Passed tests

After we've tested the register method, let's add a second spec to assert that the auth state is reset when registration fails on the server.

11. Begin by including the following import:

```
import { authState }
from 'src/app/reactive';
```

Then, update the corresponding it() function, as follows:

```
it('should reset auth state when registration fails on server ', (done) => {});
```

Add the following object, which holds the initial auth state:

```
const initialState = {
  isLoggedIn: false,
  currentUser: null,
  accessToken: null
};
```

12. Spy on the private resetAuthState() method, as follows:

```
const resetAuthStateSpy =
  spyOn(service, 'resetAuthState' as never);
```