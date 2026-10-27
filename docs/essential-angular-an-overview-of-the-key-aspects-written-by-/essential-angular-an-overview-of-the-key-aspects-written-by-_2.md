# 第 3 章 NgModules

### 提供者（Providers）

我将在第 5 章《*依赖注入*》中介绍提供者和依赖注入。这里我只想指出，NgModules 可以包含提供者。并且导入模块的提供者会从左到右与目标模块的提供者合并，也就是说，如果多个导入模块定义了同一个提供者，最后一个模块胜出。

```typescript
@NgModule({
  providers: [
    Repository
  ]
})
class TalksModule {}

@NgModule({
  imports: [TalksModule]
})
class AppModule {}
```

### 引导（Bootstrap）

要在 JIT 模式下引导 Angular 应用，你需要将一个模块传递给 `bootstrapModule`：

```typescript
import {platformBrowserDynamic} from '@angular/platform-browser';
import {AppModule} from './app';

platformBrowserDynamic().bootstrapModule(AppModule);
```

这会将 `AppModule` 编译成模块工厂，然后使用该工厂实例化模块。如果你使用 AOT，可能需要自己提供工厂：

```typescript
import {platformBrowser} from '@angular/platform-browser';
import {AppModuleNgFactory} from './app.ngfactory';

platformBrowser().bootstrapModuleFactory(AppModuleNgFactory);
```

我说“可能需要”，是因为 CLI 和 WebPack 插件会替你处理这件事。必要时，它们会将对 `bootstrapModule` 的调用替换为 `bootstrapModuleFactory`。

### 懒加载

正如我之前提到的，NgModules 不仅仅是编译单元，它们也是分发单元。这就是为什么我们引导 NgModule，而不是组件——我们不分发组件，我们分发模块。这也是为什么我们也懒加载 NgModules。

```typescript
import {NgModuleFactoryLoader, Injector} from '@angular/core';

class MyService {
    constructor(loader: NgModuleFactoryLoader, injector: Injector) {
        loader.load("mymodule").then((f: NgModuleFactory) => {
            const moduleRef = f.create(injector);
            moduleRef.injector; // module injector
            moduleRef.componentFactoryResolver; // all the component factories of the lazy-loaded module
        });
    }
}
```

当应用程序运行在 JIT 模式时，加载器会编译模块；而在 AOT 模式下则不会。`@angular/core` 自带的默认加载器使用 SystemJS，但与 Angular 中的大多数东西一样，你可以提供自己的加载器。
