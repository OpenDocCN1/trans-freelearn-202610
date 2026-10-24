

## 创建删除效果

我们下一步需要做的是为删除功能创建效果。在 `anti-hero/state/anti-hero.effect.ts` 文件中，我们将添加以下代码：

```typescript
// See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-13
removeAntiHero$ = createEffect(() => {
  return this.actions$.pipe(
    ofType(AntiHeroActions.REMOVE_ANTI_HERO_API),
    mergeMap((data: { payload: string }) =>
      this.antiHeroService.deleteAntiHero(data.payload)
        .pipe(
          map(() => ({ type:
            AntiHeroActions.REMOVE_ANTI_HERO_STATE,
            antiHeroId: data.payload })),
          catchError(() => EMPTY)
        )
    )
  )
}, { dispatch: true }
);
```

在前面的代码示例中，我们可以看到我们为删除操作创建了一个新的 effect；其类型为 `REMOVE_ANTI_HERO_API`，它会调用 `AntiHeroService` 中的 `deleteAntiHero()` 函数，根据传入的 ID 进行数据删除，并在 API 成功后。

该 effect 将派发另一个 action，`REMOVE_ANTI_HERO_STATE`，我们在上一节中创建了它，用于从 store 中移除反英雄。这意味着我们从数据库中删除的数据也会从我们的 NgRx store 中删除。

## 修改派发

此功能的最后一步是修改在 `list.component.ts` 文件中派发的 action。在上一节中，我们直接在组件中调用了 `REMOVE_ANTI_HERO_STATE` action；我们将把它改为 `REMOVE_ANTI_HERO_API`，因为我们现在应该调用 effect，它将调用 API，同时也会调用 `REMOVE_ANTI_HERO_STATE` action。

让我们看一下以下代码示例：

```typescript
selectAntiHero(data: {antiHero: AntiHero, action: TableActions}) {
  switch(data.action) {
    case TableActions.Delete: {
      this.store.dispatch({type:
        AntiHeroActions.REMOVE_ANTI_HERO_API,
        payload: data.antiHero.id});
      return;
    }
    default: ""
  }
}
```

在前面的代码示例中，我们现在在列表组件中派发 effect。这将先调用 API，然后再在应用程序中更新我们的 store；我们 store 和数据库中的更改保持同步。

在下一节中，我们将实现使用副作用向应用程序添加数据的功能。

## 使用 NgRx 添加带副作用的项目

在本节中，我们将使用 NgRx 实现带副作用的添加功能。步骤与我们实现删除功能时类似。我们将逐步创建构建块，并在组件中创建派发逻辑。

### 创建 actions

我们需要做的第一步是为添加功能创建所需的操作类型和 action。要实现 actions，我们可以思考我们为删除功能创建 actions 的方式。

概念是相同的。我们需要创建两种操作类型，分别是 `ADD_ANTI_HERO_API` 和 `ADD_ANTI_HERO_STATE`。第一个类型将由调用 API 的 effect 使用，第二个类型将由修改状态以添加新创建数据的 reducer 使用。

创建了两个操作类型后，我们还需要使用 `createAction()` 函数为 `ADD_ANTI_HERO_STATE` 类型创建一个 action。effect 将在 API 成功调用后派发它。

让我们看一下以下代码实现：

```typescript
// See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-13

export enum AntiHeroActions {
    GET_ANTI_HERO_LIST = '[Anti-Hero] Get Anti-Hero list',
    SET_ANTI_HERO_LIST = '[Anti-Hero] Set Anti-Hero list',
    ADD_ANTI_HERO_API = '[Anti-Hero] Add Anti-Hero (API)',
    ADD_ANTI_HERO_STATE = '[Anti-Hero] Add Anti-Hero (STATE)',
    REMOVE_ANTI_HERO_API = '[Anti-Hero] Remove Anti-Hero (API)',
    REMOVE_ANTI_HERO_STATE = '[Anti-Hero] Remove Anti-Hero (STATE)',
}

export const addAntiHeroState = createAction(
    AntiHeroActions.ADD_ANTI_HERO_STATE,
    props<{ antiHero: AntiHero }>()
)
```

在前面的代码示例中，我们可以看到我们在 `AntiHeroActions` 中添加了两种新的类型。我们还创建了一个新的 action，其类型为 `ADD_ANTI_HERO_STATE`，它接受一个 `antiHero` 属性，该属性将被推入反英雄状态的新项目中。

### 创建 effect

我们下一步需要做的是为添加功能创建 effect。在 `anti-hero/state/anti-hero.effect.ts` 文件中，我们将添加以下代码：

```typescript
// add anti-heroes to the database
addAntiHero$ = createEffect(() => {
    return this.actions$.pipe(
        ofType(AntiHeroActions.ADD_ANTI_HERO_API),
        mergeMap((data: {type: string, payload: AntiHero}) =>
            this.antiHeroService.addAntiHero(data.payload)
            .pipe(
                map(antiHeroes => ({ type:
                    AntiHeroActions.ADD_ANTI_HERO_STATE,
                    antiHero: data.payload })),
                tap(() =>
                    this.router.navigate(["anti-heroes"])),
                catchError(() => EMPTY)
            )
        )
    )
}, {dispatch: true})
```

在前面的代码示例中，我们可以看到我们创建了一个与删除功能 effect 类似的 effect。该 effect 使用 `ADD_ANTI_HERO_API` 类型并调用 `antiHeroService` 中的 `addAntiHero()` 函数，以调用 POST API 将新数据添加到数据库中。

成功调用 POST API 后，effect 将派发 `ADD_ANTI_HERO_STATE` action 并传递来自 API 响应的新反英雄数据，以便由 reducer 添加。我们还添加了一个 `tap` 操作符，它调用一个 `navigate` 函数，在创建新反英雄后将导航到列表页面。

### 创建 reducer

创建 effects 后，我们需要将数据库中实现的更改与我们的 store 同步，而 reducer 将执行此操作。

让我们看一下以下代码实现：

```typescript
export const antiHeroReducer = createReducer(
  initialState,
  on(setAntiHeroList, (state, { antiHeroes }) => {
    return { ...state, antiHeroes };
  }),
  on(removeAntiHeroState, (state, { antiHeroId }) => {
    return {
      ...state,
      antiHeroes: state.antiHeroes.filter(data => data.id != antiHeroId)
    };
  }),
  on(addAntiHeroState, (state, { antiHero }) => {
    return {
      ...state,
      antiHeroes: [...state.antiHeroes, antiHero]
    };
  }),
);
```

在前面的代码示例中，我们可以看到我们为 *添加* 功能添加了一个新的 reducer。它接受来自 `addAntiHeroState` action 的新反英雄数据，并返回具有修改后 `antiHeroes` 值的新状态，其中新反英雄已添加到数组中。

如果 reducer 成功修改了 `antiHeroes` 状态的值，任何订阅此状态更改的选择器都将在组件中发出新值。

### 派发 action

我们需要做的最后一步是在组件中派发 action。为了实现此步骤，我们将在反英雄表单中的 **Create** 按钮被点击时调用派发 action。在 `anti-hero/components/anti-hero-form.component.ts` 文件中，我们添加了一个 emitter，它传递表单的值和按钮标签以识别操作是创建还是更新。

让我们回顾一下我们为此反英雄表单实现的代码：

```typescript
export class AntiHeroFormComponent implements OnInit {
  @Input() actionButtonLabel: string = 'Create';
  @Output() action = new EventEmitter();
  form: FormGroup;
  constructor(private fb: FormBuilder) {
    this.form = this.fb.group({
      id: [''],
      firstName: [''],
      lastName: [''],
      house: [''],
      knownAs: ['']
    })
  }
  emitAction() {
    this.action.emit({value: this.form.value,
      action: this.actionButtonLabel})
  }
}
```

在前面的代码示例中，我们可以看到反英雄表单发出表单值作为反英雄对象，该对象将被传递给 effect。

这也提供了当前操作，因为我们也将使用此反英雄表单组件进行更新。按钮被点击后，我们需要在 `form.component.ts` 文件中有一个函数来派发 effect。

