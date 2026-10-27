# Resolution

And this is how the dependency resolution algorithm works:

```
// this is pseudocode.
let inj = this;
while (inj) {
    if (inj.has(requestedDependency)) {
        return inj.get(requestedDependency);
    } else {
        inj = inj.parent;
    }
}
throw new NoProviderError(requestedDependency);
```

When resolving the backend dependency of TalksCmp, Angular will start with the injector of the talks component itself. Then, if it is unsuccessful, it will climb up to the injector of the app component, and, finally, will move up to the injector created from AppModule. That is why, for TalksAppBackend to be resolved, you need to register it at TalkCmp, AppCmp, or AppModule.