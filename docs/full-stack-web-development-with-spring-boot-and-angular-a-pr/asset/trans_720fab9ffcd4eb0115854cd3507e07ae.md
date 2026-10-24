

# 选择特性状态

在选择特性状态时，我们将使用 `createSelector()` 和 `createFeatureSelector()` 函数来创建选择器。让我们看一个选择器选择位于特性状态（BlogState）下的博客列表的示例：

// blogs.selectors.ts
import { createSelector, createFeatureSelector } from '@ngrx/store';

export const selectBlogsState =
createFeatureSelector<BlogsState>('blogsState')

export const selectBlogs = () => createSelector
  selectBlogsState,
  (state: BlogsState) => state.blogs
)
```

在前面的代码示例中，第一步是创建特性选择器，它将返回整个 BlogState。在这里，我们使用了 `createFeatureSelector()` 和状态的键来标识我们想要选择的目标特性状态。

第二步是创建博客切片的主选择器；`createSelector()` 有两个参数，其中第一个是 BlogState 特性选择器，第二个是一个函数，其中特性选择器返回的 BlogState 是参数。返回值是博客切片。

## 在项目中实现选择器

到目前为止，我们已经学习了如何使用 `createFeatureSelector()` 和 `createSelector()` 函数创建选择器。现在，让我们在项目中实现它们。首先需要识别的是状态或切片。第一个需要选择器的切片是位于 AntiHeroState 下的博客切片。请记住 AntiHeroState 不是我们的根状态；这意味着我们将为以下状态拥有一个特性选择器。

我们需要的第二个选择器是 antiHeroes 切片的选择器，其中包含通过 API 获取的反英雄数组。最后，我们想要的第三个选择器将需要根据 id 参数从列表中选择特定的 antiHero 数据。

要创建所有这些选择器，请将以下代码放在 `anti-hero/state/anti-hero.selectors.ts` 文件中：

// select the AntiHeroState
export const selectAntiHeroState =
createFeatureSelector<AntiHeroState>('antiHeroState')

// selecting all antiheroes
export const selectAntiHeroes = () => createSelector(
  selectAntiHeroState,
  (state: AntiHeroState) => state.antiHeroes
)
// selecting an antihero base on id
export const selectAntiHero = (id: string) => createSelector(
  selectAntiHeroState,
  (state: AntiHeroState) => state.antiHeroes.find(d =>
    d.id === id)
)
```

成功创建所有选择器后，我们可以在 `anti-hero/pages/list.component.ts` 文件中使用它们，通过添加以下代码：

```
antiHeroes$ = this.store.select(selectAntiHeroes());
constructor(
  private router: Router,
  private store: Store<AppState>,
) { }
ngOnInit(): void {
  this.assignAntiHeroes();
}
assignAntiHeroes() {
  this.antiHeroes$.subscribe((data) => {
    this.antiHeroes = data;
  });
}
```

在前面的代码示例中，我们使用了 `selectAntiHeroes()` 选择器从状态中获取反英雄数组。`antiHeroes$` 是一个 Observable，在订阅后返回 `antiHero` 切片的当前状态。

最后，我们必须获取反英雄列表特性。我们可以通过在列表组件的 `ngOnInit()` 钩子中分发 `GET_ANTI_HERO_LIST` 操作来实现。这将调用我们之前创建的效果，该效果调用获取反英雄列表的端点：

```
ngOnInit(): void {
  this.store.dispatch({type: AntiHeroActions.GET_ANTI_HERO_LIST});
  this.assignAntiHeroes();
}
```

至此，我们已成功为组件创建了选择器，以便从状态中检索数据。在下一节中，我们将讨论可为 store 实现的可用配置。

## 配置 store

在前面的章节中，我们创建了 NgRx 的所有构建块，完成了应用程序的完全功能 store。在本节中，我们将学习如何使用运行时检查配置 NgRx store。

## 运行时检查

运行时检查用于配置 NgRx store，以便开发人员遵循 NgRx 和 Redux 核心概念和最佳实践。这对于刚接触 NgRx 的开发人员尤其有用；它们会根据激活的运行时检查显示与开发相关的错误。

`@ngrx/store` 提供了六个内置运行时检查：

- `strictStateImmutability`: 检查状态是否未被修改（默认值：开启）
- `strictActionImmutability`: 检查操作是否未被修改（默认值：开启）
- `strictStateSerializability`: 检查状态是否可序列化（默认值：开启）
- `strictActionSerializability`: 检查操作是否可序列化（默认值：关闭）
- `strictActionWithinNgZone`: 检查操作是否在 NgZone 内分发（默认值：关闭）
- `strictActionTypeUniqueness`: 检查注册的操作类型是否唯一（默认值：关闭）

要更改运行时检查的默认配置，我们将使用根 store 配置对象上的 `runtimeChecks` 属性。每个运行时检查的值可以通过赋值为 `true` 来激活检查，或赋值为 `false` 来停用检查：

```
@NgModule({
  imports: [
    StoreModule.forRoot(reducers, {
```

```
```javascript
runtimeChecks: {
  strictStateImmutability: true,
  strictActionImmutability: true,
  strictStateSerializability: true,
  strictActionSerializability: true,
  strictActionWithinNgZone: true,
  strictActionTypeUniqueness: true,
},
}),
],
})
```
```

### strictStateImmutability

这是 NgRx 的第一条规则。它默认激活，运行时检查会验证开发人员是否修改了状态对象。

**违反此规则的示例：**

```
```javascript
export const reducer = createReducer(initialState, on(addBlog,
  (state, { blog }) => ({
  // Violation 1: we assign a new value to loading
  state.loading = false,
    // Violation 2: `push` modifies the array
    // state.blogs.push(blog) })) );
```
```

**修复此违规：**

```
```javascript
export const reducer = createReducer( initialState, on(addBlog,
  (state, { blog }) =>
  // Fix: We are returning the state as a whole object with
  // the new values
    ({ ...state,
      loading: false,
      blogs: [...state.blogs, blog],
    })) );
```
```

### strictActionImmutability

此运行时检查与 `strictStateImmutability` 类似，但针对的是操作。此运行时检查验证开发人员是否修改了操作。此检查默认激活。

**违反此规则的示例：**

```
export const reducer = createReducer(initialState, on(addBlog,
(state, { blog }) => ({
// Violation: it's not allowed to modify an action
blog.id = uniqueID();
return { ...state, blogs: [...state.blogs, blog]
} })) );
```

**修复此违规：**

```
//blog.actions.ts
export const addBlog = createAction( '[Blog List] Add Blog',
// Fix: we will return the object in the action with the
// new value
(description: string) =>
({ id: uniqueID(), description }) );

//blog.reducer.ts
export const reducer = createReducer(
initialState,
on(addBlog, (state, { blog }) => ({
...state,
blogs: [...state.blogs, blog],
})) );
```

### strictStateSerializability

此运行时检查验证放置在状态中的值是否可序列化。这对于持久化状态以便将来重新水合至关重要。此检查默认停用。

**违反此规则的示例：**

```
export const reducer = createReducer(
initialState,
on(addBlog, (state, { blog }) => ({
...state,
blogs: [...state.blogs, blog],
// Violation: a Date type is not a serializable value.
```

```
```typescript
createdOn: new Date()
}));
```
```

**修复此违规：**

```
```typescript
export const reducer = createReducer(
  initialState,
  on(addBlog, (state, { blog }) => ({
    ...state,
    blogs: [...state.blogs, blog],
    // Fix: We should convert the date into a JSON Object.
    createdOn: new Date().toJSON()
  }))
);
```
```

### strictActionSerializability

此运行时检查与 `strictStateSerializability` 类似，但针对操作。它检查状态是否可序列化。这由 Redux DevTools 用于调试错误。

**违反此规则的示例：**

```
```typescript
const createBlog = createAction(
  '[Blog List] Add Blog',
  blog => ({ blog,
  // Violation, a function is not serializable
  logBlog: () => { console.log(blog); }, }));
```
```

**修复此违规：**

```
```typescript
const createBlog = createAction(
  '[Blog List] Add Blog',
  // Fix: we should use props to receive parameters
  props<{blog: Blog}>()
);
```
```

### strictActionWithinNgZone

此运行时检查验证操作是否由 NgZone 内的异步任务分发。此检查默认停用。

