
在 ListComponent 的 TypeScript 文件中，我们创建了 `headers` 的定义，以及一个用于反英雄列表显示的 `antiHeroes` 示例列表。这只是临时性的，因为我们只是想测试展示组件能否成功显示反英雄列表。我们还创建了 `selectAntiHero()` 函数，以便将来在选中某个特定反英雄时实现相应功能。

现在让我们为 AntiHeroList 定义输入和输出属性。让我们看看下面的代码：

## list.component.html

```
<!-- Dumb component anti hero list -->
<app-anti-hero-list [antiHeroes]="antiHeroes" [headers]="headers" (antiHero)="selectAntiHero($event)"></app-anti-hero-list>
```

现在，在 ListComponent 的 HTML 文件中，我们已将 `headers` 和 `antiHeroes` 绑定到 `app-anti-hero-list` 的属性上。我们还使用了 `selectAntiHero()` 函数，以便在 `antiHero` 发出操作时捕获该事件。

成功实现我们的展示组件后，我们可以运行应用程序并在浏览器中打开它。我们应该会看到以下结果：

![](img/7c3ddd2e5893c9b5a0220148ed886548_256_0.png)

从结果中我们可以看到，展示组件已成功显示了来自父组件的数据。至于表单组件，我们将在下一章中实现其功能，因为创建表单将是另一个不同的话题。

现在，我们已经了解了智能组件和哑组件的概念、结构和实现。在下一节中，我们将使用一个 UI 框架来帮助我们改进 Angular 应用程序的界面。

## 添加 Angular Material

我们已经使用模块级别的核心、功能和共享架构，以及组件级别的智能组件和哑组件架构，对 Angular 应用程序进行了组织。现在，我们准备通过为组件设置样式来自定义和改进外观与 UI。我们都知道，从零开始编写 CSS 代码并开发基础样式对我们开发人员来说是另一个挑战。除了操心代码的逻辑层面之外，这还会给我们带来额外的负担。而 Angular Material 正是大显身手的地方！

**Angular Material** 是由 Google 创建的一个库，为 Angular 应用程序提供了丰富的 UI 组件，例如表格、卡片、输入框和日期选择器。这意味着我们不必从头开始为组件设置样式，因为 Material 库中的组件列表已经可以直接使用。

Angular Material 在底层提供了大量且不断增长的组件。它提供了包含这些组件的模块，这些组件可用于 Angular 应用程序，并可导入到特定的应用程序模块中；这些组件是可复用的，并且由于内置了许多可供我们使用的属性，因此它们在外观和交互体验方面易于定制。

让我们在 Angular 项目中配置 Angular Material。

## 配置 Angular Material

Angular Material 在 Angular 项目中配置起来很简单，因为它提供了一个 schematic，只需一条命令即可安装 Angular Material 的所有依赖项。要安装 Angular Material，我们将执行以下命令：

```
ng add @angular/material
```

执行该命令后，在安装资源之前它会询问一些问题：

+   - **选择预构建主题名称，或选择“custom”作为自定义主题：** Angular Material 提供了预构建主题，你也可以配置自己的自定义主题。
+   - **设置全局 Angular Material 排版样式：** 选择“是”将应用 Angular Material 的全局排版。
+   - **为 Angular Material 设置浏览器动画：** 选择“是”将在我们的根模块中安装 `BrowserAnimationsModule`。当我们想要使用 Angular Material 的动画时，这很重要。

回答完所有问题后，它现在将安装 Angular Material。这将为我们的项目执行以下操作：

+   1. 将依赖项（`@angular/material` 和 `@angular/cdk`）添加到 `package.json`。
+   2. 将 Roboto 字体添加到 `index.html` 文件：
+   3. 将 Material Design 图标字体添加到 `index.html` 文件：

```
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap" rel="stylesheet">
```

```
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

+   4. 添加以下 CSS 样式：
+     - 将 `html` 和 `body` 的高度设置为 100%
+     - 将 Roboto 设置为默认字体
+     - 移除 `body` 的外边距：

```
html, body { height: 100%; }
body { margin: 0; font-family: Roboto, "Helvetica Neue", sans-serif; }
```

在我们的应用程序中成功安装 Angular Material 后，我们现在就可以在应用程序中使用这些组件了。

## 实现 Angular Material

我们现在将在 Angular 项目中实现 Angular Material 组件。正如我们之前提到的，Angular Material 提供了大量可用于构建应用程序的组件。在这个示例项目中，我们将只定义应用程序中会使用到的组件。让我们列出将要实现的组件。

### 按钮组件

使用 Material Design 样式增强的原生 `<button>` 或 `<a>` 元素。

我们可以使用以下代码导入按钮组件：

```
import {MatButtonModule} from '@angular/material/button';
```

以下是按钮组件的示例：

```
<div class="example-button-row">
    <button mat-raised-button>Basic</button>
    <button mat-raised-button
        color="primary">Primary</button>
    <button mat-raised-button
        color="accent">Accent</button>
    <button mat-raised-button color="warn">Warn</button>
    <button mat-raised-button disabled>Disabled</button>
    <a mat-raised-button href="
        https://material.angular.io/target=" _blank>Link</a>
</div>
```

在上面的 Material 按钮代码示例中，我们可以看到我们正在使用 Material Design 的内置指令来改变按钮的样式和外观。该示例代码将产生以下输出：

![](img/7c3ddd2e5893c9b5a0220148ed886548_259_0.png)

图 10.10 – Material 按钮的示例输出

### 图标组件

该组件允许我们向应用程序添加基于矢量的图标，并且同时支持图标字体和 SVG 图标。

我们可以使用以下代码导入图标组件：

```
import {MatIconModule} from '@angular/material/icon';
```

以下是图标组件的示例：

```
<mat-icon color="primary">delete</mat-icon>
<mat-icon color="accent">fiber_new</mat-icon>
<mat-icon color="warn">pageview</mat-icon>
```

在上面的代码示例中，我们可以使用 `mat-icon` 组件创建图标。它有几个输入属性（例如 `color`），允许我们自定义图标的颜色。该示例代码将产生以下输出：

![](img/7c3ddd2e5893c9b5a0220148ed886548_259_1.png)

图 10.11 – Material 图标的示例输出

### 表格组件

该组件允许我们添加具有 Material Design 样式的数据表格。Material 表格基于 CDK 数据表格的基础构建。有关如何实现 CDK 数据表格的更多信息，请参阅 https://material.angular.io/cdk/table/overview 上的文档。

我们可以使用以下代码导入表格组件：

```
import {MatTableModule} from '@angular/material/table';
```

以下是表格组件的示例：

```
```html
<table mat-table [dataSource]="data" class="mat-elevation-z8">

  <!-- Position Column -->
  <ng-container matColumnDef="id">
    <th mat-header-cell *matHeaderCellDef> ID </th>
    <td mat-cell *matCellDef="let element">
      {{element.position}} </td>
  </ng-container>

  <!-- Name Column -->
  <ng-container matColumnDef="name">
    <th mat-header-cell *matHeaderCellDef> Name </th>
    <td mat-cell *matCellDef="let element">
      {{element.name}} </td>
  </ng-container>

  <tr mat-header-row *matHeaderRowDef="columns"></tr>
  <tr mat-row *matRowDef="let row; columns: columns;"></tr>
</table>
```
```

在上面的示例中，我们可以看到表格使用了几个属性。第一个属性是 `dataSource` 属性，它将接受要显示的数据列表。下一个属性是 `matColumnDef`，它定义了每一列的字段名，这些字段名应包含在绑定到 `matHeaderRowDef` 属性的 `columns` 变量中。最后，`matHeaderCellDef` 和 `matCellDef` 属性会显示实际的列名和关联值，如下面的截图所示：

![](img/7c3ddd2e5893c9b5a0220148ed886548_260_0.png)

图 10.12 – Material 表格的示例输出

### 工具栏组件

该组件允许我们添加具有 Material Design 样式的工具栏。它通常用作页眉、标题和导航按钮的容器。

