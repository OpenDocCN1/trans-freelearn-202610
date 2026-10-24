

## 12
## 使用 NgRx 管理状态

在上一章中，我们学习了响应式表单的概念和特性。我们实现了 `FormGroup`、`FormBuilder` 和 `formControlName` 来在应用程序的表单元素中绑定输入值。我们还讨论了如何对表单控件进行分组以绑定嵌套属性，并在响应式表单中创建表单数组。

之后，我们学习了如何为表单控件实现验证以处理和验证用户输入。这将有利于用户体验并帮助我们避免意外的错误。

在本章中，我们将向我们的应用程序添加一个新的食谱，并学习如何实现状态管理，特别是 **NgRx**，以处理 Angular 应用程序中的数据。

在本章中，我们将涵盖以下主题：

- 理解管理大型应用程序状态的复杂性
- 状态管理与全局状态
- 开始使用并设置 NgRx
- 编写一个 action
- 编写一个 effect
- 编写一个 reducer
- 编写一个 selector 并在组件中使用 selector 并分派它
- 配置 store

## 技术要求

以下链接将带您进入本章代码的完成版本：https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-12/superheroes。

在前面的图形说明中，我们可以看到我们有四个不同的服务负责管理不同类型的数据；每个功能都从这些服务中检索其所需的数据。

正如我们所见，检索是分散的，因为功能从不同的源或服务检索数据。这导致了多个数据流，并且随着需要更多服务和功能而变得更大。

这也可能导致每个组件持有的数据不一致，因为来源来自不同的服务，从而导致应用程序中出现一些意外的错误。现在，让我们来看一下使用状态管理的数据流。

## 使用状态管理的数据流

在之前的数据流中，我们看到数据流未得到优化，这可能导致应用程序出现若干问题，因为数据流的方向非常复杂。在 Angular 应用程序中实现状态管理时，我们将具有以下数据流：

![](img/7c3ddd2e5893c9b5a0220148ed886548_297_0.png)

图 12.2 – 使用状态管理的数据流

在前面的图形说明中，我们可以看到应用程序的所有服务都由状态管理处理。我们仍然使用服务从数据库中检索数据。这里的显著区别在于，我们的功能现在是在状态中访问所有检索到的数据，而不是直接在服务中访问它。

这允许数据单向流动，并且所有在应用程序中使用的数据的来源只有一个。通过这种方法，可以避免状态不一致、可能的错误和多次 API 调用。

有了这些，我们已经了解了状态管理在开发应用程序，特别是在企业级应用程序中的重要性。

现在，让我们讨论更多关于状态管理概念和全局状态的内容。

## 状态管理与全局状态

正如前一节所讨论的，状态管理是一个扩展或库，它允许我们在应用程序中以单一方向管理数据的流动。

这是由于全局状态的存在，全局状态将包含所有数据。为了了解状态管理的工作原理，让我们讨论状态管理的每个构建块。

### 全局状态/store

**全局状态**，也称为 **store**，是状态管理中最关键的元素。全局状态的主要职责是存储 API 检索到的所有数据，或简单地在应用程序中使用的所有数据。

这意味着 Angular 应用程序中的所有组件都将在全局状态中检索数据。将其视为 Angular 应用程序的数据库，但以 JSON 对象的形式存在，我们可以在其中获取每个属性作为切片。

### Actions

**Actions** 表示应用程序中的唯一事件。它们直接在我们的组件中使用 `dispatch()` 函数调用，这有助于识别应执行哪些事件，例如修改状态或调用 API 以检索数据。

Actions 只是简单的接口；`type` 属性标识分派了什么 action。这个简单的字符串只是 action 的定义，我们可以向 actions 添加属性以获取 API 或状态所需的数据。

让我们看一个 action 接口的示例：

```
{
  type: '[Blog] Add Blog',
  title: string;
  author: string;
  content: string;
}
```

前面的示例 action 在创建新博客时被分派。这是在点击 **Submit** 按钮时调用的，并且诸如 `title`、`author` 和 `content` 之类的信息被添加为附加元数据以传递给 effect 或 reducer。

### Reducers

**Reducers** 是状态管理的决策者。它们根据 action 类型决定处理哪些 action。Reducers 也是可以更改状态值的对象。

Reducers 是纯函数并同步处理状态转换；让我们看一个 reducer 的示例：

```
```typescript
export const blogReducer = createReducer( initialState,
  on(BlogActions.addBlog, (state, {blog}) => ({ ...state,
    blogs: [...state.blogs, blog]})
  );
```
```

在前面的示例中，我们为 `addBlog()` action 创建了一个 reducer。这允许我们在分派 `addBlog()` action 后在博客的状态中添加一个新的博客对象。

我们将在本章的 *Writing a reducer* 部分更详细地讨论 reducers。

### Selectors

**Selectors** 是纯函数，允许我们检索 store 中的数据切片。它是一种变更检测机制，当状态的值发生变化时，它会比较状态的部分，并且只有在检测到更改时才发送状态。这是一种称为 **memorization** 的做法。

Selectors 在组件中使用以获取 UI 中使用的数据。它作为 Observable 返回，监听状态变化。

让我们看一个 selector 的示例：

```
```typescript
// selector for list of blogs
// blog.selector.ts
export const selectBlogList = (state: AppState) => state.blogs;

// blog component
// blog.component.ts
blogs$ = this.store.select<Array<Blog>>(selectBlogList);
  this.blogs$.subscribe(data => {
```
```

```
console.log(data) // list of blogs from the state;
});
```

在前面的示例中，我们为博客切片创建了一个 selector，以及一个返回博客元数据的函数，称为 selectBlogList()。我们在博客组件中使用此函数作为 select 函数的参数以通过订阅 selector 检索数据。订阅将在博客切片的值发生变化时发出。我们将在本章的下一节更详细地讨论 selectors。

### Effects

Effects 是 NgRx 库使用的特定元素；这是一个由 RxJs 驱动的副作用模型，处理外部交互，例如 API 调用、WebSocket 消息和基于时间的事件。使用 effects，我们可以将组件与外部源的交互隔离并减少其职责。让我们看一下有副作用和没有副作用的应用程序之间的比较。

### 没有副作用的应用程序

以下是没有副作用的应用程序：

```
export class BlogPageComponent {
  movies: Blog[];
  constructor(private blogService: MoviService) {}
  ngOnInit() {
    this.blogService
      .getAll()
      .subscribe(blogs => this.blogs = blogs);
  }
}
```

在前面的代码示例中，我们有一个组件具有以下几个职责：

- 管理博客的状态（组件具有其博客状态）
- 使用博客服务调用外部 API 以获取博客列表
- 在组件内部修改博客的状态

这意味着每个具有服务依赖的组件也都有其数据状态。现在，让我们看一个有副作用的应用程序的示例。

