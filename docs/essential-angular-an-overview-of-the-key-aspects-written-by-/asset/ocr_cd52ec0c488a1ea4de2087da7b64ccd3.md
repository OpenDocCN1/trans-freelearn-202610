# Lazy loading

As I mentioned earlier NgModules are not just the units of compilation, they are also the units of distribution. That is why we bootstrap a NgModule, and not a component—we don't distribute components, we distribute modules. And that's why we lazy load NgModules as well.

```
import {NgModuleFactoryLoader, Injector} from '@angular/core';

class MyService {
    constructor(loader: NgModuleFactoryLoader, injector: Injecto
    loader.load("mymodule").then((f: NgModuleFactory) => {
        const moduleRef = f.create(injector);
        moduleRef.injector; // module injector
        moduleRef.componentFactoryResolver; // all the \
components factories of the lazy-loaded module
    });
    }
}
```

The loader compiles the modules if the application is running in the JIT mode, and does not in the AOT mode. The default loader @angular/core ships with uses SystemJS, but, as most things in Angular, you can provide your own.