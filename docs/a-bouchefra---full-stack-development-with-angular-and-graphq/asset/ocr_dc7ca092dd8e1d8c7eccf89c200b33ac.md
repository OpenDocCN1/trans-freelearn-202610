```
import { environment } from './environments/environment';

if (environment.production) {
    enableProdMode();
}
// [...]
```

This simply enables the production mode if the environment.production variable is set to true, which is the case in the environments/environment.prod.ts file used by the Angular CLI when you run the ng build --prod command.

Go ahead and select some of the DOM elements in your application, for example, the Feed button, and then right-click and click on Inspect (or Ctrl + Shift + I on Chrome) in the context menu.

Inside DevTools, we can reference the most recently selected DOM element using $0. Combining that with the ng.getOwningComponent() method, we can get the component that rendered that element in the console, as follows:

```
> ng.getOwningComponent($0)
< ▼ HeaderComponent { _ngContext_: LComponentView_AppComponent(53) } ⓘ
  ► _ngContext__: LComponentView_AppComponent(53) [app-root, TView, 147, LRootView(31), nul...
  ▼ _proto__:
    ► constructor: class HeaderComponent
    ► ngOnInit: f ngOnInit()
    ► _proto__: Object
> |
```

Figure 6.3 – Getting the parent component of a DOM element using ng

Go back to the src/app/core/components/header/header.component.ts file and add the following method to the component:

```
invokeMe() {
    console.log("Invoked from the console");
}
```

Next, go back to the running application, select a button from the header area, run the ng.getOwningComponent($0) method, and assign the result to a component variable, as follows:

```
const component = ng.getOwningComponent($0)
```