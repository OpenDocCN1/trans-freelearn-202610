

## 模块

**模块**是 Angular 框架的基本特性之一。随着我们的应用变得越来越复杂，它将由大量组件、指令和服务组成的大型代码块构成。这将影响应用代码库的可维护性。Angular 框架提供了一种组织和分组代码块的方式，称为模块。

Angular 框架中的模块帮助我们开发应用，并促进关注点分离。它们使我们能够根据功能对代码块进行分类和组织。Angular 本身也是使用模块构建的；`@angular/core` 框架是主要的 Angular 模块，它提供了 Angular 的核心功能和服务。

### 创建模块

我们将使用 `@NgModule` 装饰器来创建一个模块。它由多种类型的元数据组成，这些元数据使我们能够定义所创建模块中包含的组件、服务、管道和其他模块。

下面的示例代码展示了模块可用的属性：

```
@NgModule({
  declarations: [],
  imports: [],
  providers: [],
  exports: [],
  bootstrap: [],
  entrycomponents: []
})
```

现在让我们讨论每个属性的功能：

- **声明（Declarations）**：这是放置应用组件、指令和管道的地方。请记住，组件、指令和管道必须只在一个模块中声明。
- **提供者（Providers）**：这是放置服务的地方，以便它们可用于依赖注入。
- **导入（Imports）**：这是放置应用中的一个或多个其他模块的地方。一旦我们导入某个特定模块，导入模块中的所有组件、管道和指令都可以使用。
- **导出（Exports）**：这是放置组件、指令和管道的地方，以便在导入后可供其他模块使用。
- **引导（Bootstrap）**：这是放置模块主组件的地方。Bootstrap 属性主要用于 Angular 应用的根模块（`AppModule`），因为根模块的职责是在应用启动时加载第一个视图。

- **入口组件（Entry components）**：这是放置应用中应动态加载的组件的地方。

下图展示了 NgModule 在 Angular 应用中的工作方式：

![](img/7c3ddd2e5893c9b5a0220148ed886548_239_0.png)

图 10.5 – Angular 模块的流程图

### 服务与依赖注入

**服务**也是 Angular 的宝贵特性之一。它是在应用的不同组件中可以重复使用的代码。

服务的主要职责如下：

- 在不同组件中复用逻辑
- 实现 API 通信和数据访问
- 促进单一职责，因为它将组件的独立功能分离开来

要在应用中创建服务，我们将创建一个类，并使用 @Injectable 装饰器对其进行注解。要在应用的根级别注册服务，我们将在 @Injectable 装饰器中添加以下内容：

```
@Injectable({
  providedIn: 'root',
})
```

一旦我们在根级别设置了 providedIn 元数据的值，这将在整个应用中创建该服务的单个共享实例。另一方面，如果我们想在特定模块中提供服务，我们将把服务放在 @NgModule 的 providers 元数据中：

```
@NgModule({
  providers: [Service1]
})
```

既然我们已经讨论了 Angular 的一些基本特性，接下来我们将重点讨论如何构建 Angular 应用的结构。

### 创建 Angular 文件夹结构

Angular 框架被视为一个 MVW 框架，这意味着我们的应用有许多可能的结构方式。在这里，我们将讨论一种最佳实践或最常用的结构，它可以帮助你的 Angular 应用具有可扩展性和可维护性。

在前一部分中，我们了解到 Angular 代码块可以分组并组织到模块中；模块是构建 Angular 应用结构的一个很好的起点。我们可以实施的第一步是根据功能对模块进行分组和分类。下面列出的是我们的模块分类。

### 根模块

**根**模块是应用的主模块。这是 Angular 应用启动时加载的第一个模块。当我们搭建一个名为 `AppModule` 的 Angular 应用时，根模块会自动生成，并位于 `src/app` 文件夹下。

### 特性模块

**特性**模块是我们放置应用特定功能模块的地方。这意味着我们代码使用的大部分内容都在这个模块中。我们将在模块下创建组件、管道和指令，还可以通过将带有路由的组件放在页面文件夹中来分离组件。

让我们来看一个名为 `AntiHeroModule` 的特性模块的示例文件夹结构：

```
src
├─ app
│  ├─ anti-hero
│  │  ├─ components
│  │  │  ├─ shared.component.ts
│  │  ├─ directives
│  │  │  ├─ first.directive.ts
│  │  │  ├─ another.directive.ts
│  │  ├─ pages
│  │  │  ├─ form
│  │  │  │  ├─ form.component.ts
│  │  │  ├─ list
│  │  │  │  ├─ list.component.ts
│  │  │  ├─ anti-hero.component.ts
│  │  │  ├─ anti-hero.component.html
│  │  │  ├─ anti-hero.component.css
│  │  │  ├─ index.ts
│  │  ├─ pipes
│  │  │  ├─ first.pipe.ts
│  │  ├─ anti-hero.module.ts
│  │  ├─ anti-hero.routing.module.ts
│  │  ├─ index.ts
```

在这里的文件夹结构中，我们将 anti-hero 模块分成了几个部分。第一个文件夹是 components 文件夹，它包含此模块中共享的所有组件。这些也可以称为哑组件（dumb components），我们将在下一节讨论。

接下来的两个是 directives 和 pipes 文件夹，它们包含 AntiHeroModule 中使用的指令和管道。最后，pages 文件夹包含 AntiHeroModule 中具有直接路由的组件。这些也可以称为智能组件（smart components）。我们还包含了 index.ts 文件，即 barrel 文件，它将为导出组件、指令和管道提供一个集中的位置。

### 共享模块

共享模块是在整个应用中使用和共享的模块；它由我们在应用不同部分需要使用的组件、管道和指令组成。请记住，共享模块不应依赖应用中的其他模块。

共享模块创建在 src/app/shared 文件夹下。

让我们看一下应用中共享模块的示例文件夹结构：

```
|— src
| |— app
| | |— shared
| | | |— layout
| | | | |— footer
| | | | | |— footer.component.ts
| | | | | |— footer.component.html
| | | | |— header
| | | | | |— header.component.ts
| | | | | |— header.component.html
| | | | |— layout.module.ts
| | | |— index.ts
```

### 特性模块下的代码块

我们想要在特性模块下创建代码块；首先需要搭建的是 AntiHeroModule。执行 ng g m anti-hero 命令，在 src/app 文件夹下生成该模块。

现在，在 anti-hero 文件夹下，我们将创建以下文件夹：

- components：将包含在此模块中共享的组件。
- pipes：将包含 anti-hero 模块使用的所有管道。
- directives：将包含 anti-hero 模块将使用的所有指令。
- pages：将包含具有直接路由的组件。

创建完这些文件夹后，我们现在将为 anti-hero 模块创建页面组件。我们将添加两个页面，第一个页面用于显示 anti-hero 列表，第二个页面是一个表单，允许我们查看、创建或修改列表中所选英雄。我们可以执行 ng g c anti-hero/pages/pages/form 和 ng g c anti-hero/pages/list 命令来创建这两个页面。这将在 pages 文件夹下创建两个新组件：form 和 list。

成功创建页面组件后，我们还将为 anti-hero 模块添加一个路由模块。我们将执行 ng g m anti-hero/anti-hero-routing --flat 命令，并将以下代码放入路由模块中：

```
import {NgModule} from "@angular/core";
import {RouterModule, Routes} from "@angular/router";
import {ListComponent} from "./pages/list/list.component";
import {FormComponent} from "./pages/form/form.component";

const routes: Routes = [
  {
    path: "",
    component: ListComponent,
  },
  {
    path: "form",
    component: FormComponent,
  },
];
@NgModule({
```

