## 13

## 使用 NgRx 保存、删除和更新

在上一章中，我们学习了 NgRx 的概念和功能。我们学习了状态管理的重要性，因为它为应用程序提供了一个单一的来源以实现单向数据流并减少组件的责任。我们还学习了 NgRx 的构建块，即 actions、effects、reducers 和 selectors。最后，我们在应用程序中使用 NgRx 实现了获取和显示反英雄列表功能。

在本章中，我们现在将完成应用程序中缺失的功能——仍使用 NgRx 保存、删除和更新数据。

在本章中，我们将涵盖以下主题：

- 使用 NgRx 无副作用地删除一项
- 使用 NgRx 有副作用地删除一项
- 使用 NgRx 有副作用地添加一项
- 使用 NgRx 有副作用地更新一项

## 技术要求

完成版代码的链接是 https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-13/superheroes。

## 使用 NgRx 无副作用地删除一项

在本节中，我们将首先了解如何在 NgRx 中不使用副作用删除项目。正如我们在上一章中所学，副作用用于调用外部 API 以检索数据。这意味着在不使用 effects 的情况下，我们将通过分派 action 来调用基于分派类型的 reducer 来删除数据。本节将帮助我们了解在应用程序中使用 effects 的流程和行为差异。

### 创建删除 action

第一步是为删除功能创建 action。在我们的项目中，在 `anti-hero/state/anti-hero.actions.ts` 文件中，我们将添加一个新的 action 接口和一个新的删除函数。

让我们看一下以下代码的实现：

```
```typescript
export enum AntiHeroActions {
    GET_ANTI_HERO_LIST = '[Anti-Hero] Get Anti-Hero list',
    SET_ANTI_HERO_LIST = '[Anti-Hero] Set Anti-Hero list',
    REMOVE_ANTI_HERO_STATE =
      '[Anti-Hero] Remove ALL Anti-Hero (STATE)',
}
export const removeAntiHeroState = createAction(
    AntiHeroActions.REMOVE_ANTI_HERO_STATE,
    props<{ antiHeroId: string }>()
);
```
```

在前面的代码示例中，我们可以看到我们添加了一个名为 `REMOVE_ANTI_HERO_STATE` 的新 action。我们还创建了一个具有新创建类型的 action，该 action 具有 `props` 参数，可接受反英雄 ID。ID 是 reducer 识别我们应该从 store 中删除哪些数据的必需信息。

### 创建删除 reducer

现在，让我们为从 store 中删除数据创建 reducer。首先需要考虑的是，如果 reducer 能够使用提供的 ID 从数组中删除单个数据片段，它会是什么样子。我们实现此功能的一种方法是使用 `filter()` 函数来提取数组中的数据。

让我们在 `anti-hero/state/anti-hero.reducers.ts` 文件中添加以下代码：

```
```typescript
export const antiHeroReducer = createReducer(
    initialState,
    on(setAntiHeroList, (state, { antiHeroes }) => { return
        {...state, antiHeroes}),
    on(removeAntiHeroState, (state, { antiHeroId }) => {
        return {...state, antiHeroes:
            state.antiHeroes.filter(data => data.id !=
```
```
```
antiHeroId } }
}) ,
);
```

在前面的代码示例中，我们可以看到我们为删除功能添加了一个新的 reducer。它接受来自 removeAntiHeroState action 的反英雄 ID，并返回具有修改后 antiHeroes 值的新的 state，其中具有给定 ID 的反英雄数据已被过滤。如果 reducer 成功修改了 antiHeroes state 的值，则订阅此 state 更改的任何 selector 都会在组件中发出新值。

### 分派 action

我们需要做的最后一步是在组件中分派 action。要实现此步骤，我们需要在单击每个反英雄数据的 Delete 按钮时调用 dispatch。

在 anti-hero/components/anti-hero-list.component.ts 文件中，我们添加了 emittethatch，它传递所选的反英雄对象和 TableAction，基于用户单击的按钮。

让我们回顾一下我们在以下文件中为此功能实现的代码：

### anti-hero-list.component.ts

```
// See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-13 /
export class AntiHeroListComponent implements OnInit {
  // other code for component not displayed
  @Output() antiHero = new EventEmitter<{antiHero: AntiHero, action :TableActions}>();
  selectAntiHero(antiHero: AntiHero, action: TableActions) {
    this.antiHero.emit({antiHero, action});
  }
}
```

### anti-hero-list.component.html

```
// See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-13
```

```
<button (click)="selectAntiHero(element, 1)" mat-raised-button color="warn">
    <mat-icon>delete</mat-icon> Delete
</button>
```

### table-actions.enum.ts

```
export enum TableActions {
    View,
    Delete
}
```

在前面的代码示例中，我们可以看到如果单击 Delete 按钮，这将发出整个反英雄对象和值 1，该值代表 Delete 枚举的值。

现在，我们需要在列表组件中在 Delete 按钮发出事件时分派 REMOVE_ANTI_HERO_STATE action。要实现这一部分，我们将在以下文件中添加以下代码：

### list.component.ts

```
selectAntiHero(data: {antiHero: AntiHero, action: TableActions}) {
    switch(data.action) {
        case TableActions.Delete: {
            this.store.dispatch({type: AntiHeroActions. REMOVE_ANTI_HERO_STATE, antiHeroId: data.antiHero.id});
            return;
        }
        default: ""
    }
}
```

### list.component.html

```
// See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-13
<!-- Dumb component anti hero list -->
```

```
```html
<app-anti-hero-list [antiHeroes]="antiHeroes"
(antiHero)="selectAntiHero($event)" [headers]="headers"></app-
anti-hero-list>
```
```

在前面的代码示例中，我们创建了一个函数来检查用户触发的 action 与 TableActions 值是否匹配。如果 TableActions 具有 delete 值，我们将分派 REMOVE_ANTI_HERO_STATE 并传递将被我们创建的 reducer 使用的反英雄对象的 ID。

我们现在已成功使用 NgRx 实现了应用程序的删除功能，但在这种情况下，我们只是在 UI 中删除项目，并且没有同步数据库中的更改。

在下一节中，我们将实现在删除数据时使用副作用。

## 使用 NgRx 有副作用地删除一项

在本节中，我们将通过在 state 中添加 effects 来改进删除功能。我们当前的删除功能仅从 store 中删除数据，但不会同步数据库中的更改。这意味着如果我们刷新应用程序，我们已删除的数据将再次可用。

为了同步数据库中的更改，我们应该做的是创建一个 effect 来调用 delete API。让我们在以下各节中查看代码中的逐步更改。

### 创建新的 action 类型

我们需要做的第一步是创建新的 action 类型。NgRx 中的 effects 稍后将使用新的 action 类型用于删除功能。

我们将在 anti-hero/state/anti-hero.actions.ts 文件下的 AntiHeroActions 枚举中添加 REMOVE_ANTI_HERO_API。

让我们看一下以下代码中添加的 action：

```
```typescript
export enum AntiHeroActions {
  GET_ANTI_HERO_LIST = '[Anti-Hero] Get Anti-Hero list',
  SET_ANTI_HERO_LIST = '[Anti-Hero] Set Anti-Hero list',
  REMOVE_ANTI_HERO_API =
    '[Anti-Hero] Remove Anti-Hero (API)',
  REMOVE_ANTI_HERO_STATE =
    '[Anti-Hero] Remove Anti-Hero (STATE)',
}
```
```

我们可以在前面的代码示例中看到，为我们的 actions 添加了一个新的 action 类型。请注意，我们不需要为此类型创建新的 action，因为一旦分派此 action 类型，我们将调用 effect 而不是 action。





###### 创建删除效果

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

###### 修改派发

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

#### 使用 NgRx 添加带副作用的项目

在本节中，我们将使用 NgRx 实现带副作用的添加功能。步骤与我们实现删除功能时类似。我们将逐步创建构建块，并在组件中创建派发逻辑。

######### 创建 actions

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

######### 创建 effect

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

######### 创建 reducer

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

######### 派发 action

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





让我们来看一下下面的代码示例：

```
// form.component.html
<app-anti-hero-form [selectedAntiHero]="antiHero"
(action)="formAction($event)"></app-anti-hero-form>

// form.component.ts
formAction(data: {value: AntiHero, action: string}) {
  switch(data.action) {
    case "Create" : {
      this.store.dispatch({type:
        AntiHeroActions.ADD_ANTI_HERO_API,
        payload: data.value});
      return;
    }
    default: ""
  }
}
```

在前面的代码示例中，我们可以看到我们创建了 formAction() 函数，该函数根据从反英雄表单组件传递过来的值派发一个 action。

这使用了 switch 语句，因为当 action 为 update 时也会调用它。我们现在已经成功地使用 NgRx 的构建块为我们的应用程序创建了添加功能。

在下一节中，我们将实现带有副作用的数据修改。

#### 使用 NgRx 带副作用更新项目

在本节的最后部分，我们将实现最终缺失的功能，即更新功能，我们将逐步创建构建块，并在组件中创建派发逻辑，就像我们为添加和删除功能所做的那样。

######### 创建 actions

我们需要做的第一步是为我们的更新功能创建所需 action 类型和 actions。我们将首先创建我们需要的两个 action 类型，它们是 MODIFY_ANTI_HERO_API 和 MODIFY_ANTI_HERO_STATE。第一个类型将被调用 API 的 effect 使用，第二个类型将被 reducer 使用，该 reducer 通过根据新的反英雄对象更改数据来修改状态。

创建了两个 action 类型后，我们还需要使用 createAction() 函数为 MODIFY_ANTI_HERO_STATE 类型创建一个 action。effect 将在 API 成功调用后派发它。

让我们来看一下下面的代码实现：

```
// See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-13

export enum AntiHeroActions {
    GET_ANTI_HERO_LIST = '[Anti-Hero] Get Anti-Hero list',
    SET_ANTI_HERO_LIST = '[Anti-Hero] Set Anti-Hero list',
    ADD_ANTI_HERO_API = '[Anti-Hero] Add Anti-Hero (API',
    ADD_ANTI_HERO_STATE =
        '[Anti-Hero] Add Anti-Hero (STATE)',
    REMOVE_ANTI_HERO_API =
        '[Anti-Hero] Remove Anti-Hero (API)',
    REMOVE_ANTI_HERO_STATE =
        '[Anti-Hero] Remove Anti-Hero (STATE)',
    MODIFY_ANTI_HERO_API =
        '[Anti-Hero] Modify Anti-Hero (API)',
    MODIFY_ANTI_HERO_STATE =
        '[Anti-Hero] Modify Anti-Hero (STATE)',
}
export const modifyAntiHeroState = createAction(
    AntiHeroActions.MODIFY_ANTI_HERO_STATE,
    props<{ antiHero: AntiHero }>()
);
```

在前面的代码示例中，我们可以看到我们在 AntiHeroActions 中添加了两个新的类型。我们还创建了一个新的 action，其类型为 MODIFY_ANTI_HERO_STATE，它接受一个 antiHero 属性，该属性将用于修改 store 中的当前值。

######### 创建 effect

我们需要做的下一步是为添加功能创建 effect。在 anti-hero/state/anti-hero.effect.ts 文件中，我们将添加以下代码：

```
// modify anti-heroes in the database
    modifyAntiHero$ = createEffect(() =>{
```

```
return this.actions$.pipe(
    ofType(AntiHeroActions.MODIFY_ANTI_HERO_API),
    mergeMap((data: {type: string, payload: AntiHero}) =>
        this.antiHeroService.updateAntiHero(
            data.payload.id, data.payload)
        .pipe(
            map(antiHeroes => ({ type:
                AntiHeroActions.MODIFY_ANTI_HERO_STATE,
                antiHero: data.payload })),
            tap(() =>
                this.router.navigate(["anti-heroes"])),
            catchError(() => EMPTY)
        )
    }, {dispatch: true})
```

在前面的代码示例中，我们可以看到我们创建了一个与添加和删除功能的 effect 类似的 effect。此 effect 使用 MODIFY_ANTI_HERO_API 类型并调用 antiHeroService 中的 updateAntiHero() 函数，以调用 PUT API 来修改数据库中带有 ID 参数的反英雄。

成功调用 PUT API 后，effect 将派发 MODIFY_ANTI_HERO_STATE action 并传递来自 API 响应的修改后的反英雄数据以供 reducer 添加，与添加 effect 相同，我们还添加了一个 tap 操作符，该操作符调用 navigate 函数，在修改反英雄后将导航到列表页面。

######### 创建 reducer

创建 effects 后，我们需要将数据库中实现的变化与我们的 store 同步，reducer 将执行此操作。

让我们来看一下下面的代码实现：

```
export const antiHeroReducer = createReducer(
    initialState,
    on(setAntiHeroList, (state, { antiHeroes }) => {
        return {...state, antiHeroes}}),
    on(removeAntiHeroState, (state, { antiHeroId }) => {
        return {...state, antiHeroes:
            state.antiHeroes.filter(data => data.id !=
```

```
antiHeroId) }
}),
on(addAntiHeroState, (state, {antiHero}) => {
    return {...state, antiHeroes: [...state.antiHeroes,
        antiHero]}
}),
on(modifyAntiHeroState, (state, {antiHero}) => {
    return {...state, antiHeroes: state.antiHeroes.map(data
        => data.id === antiHero.id ? antiHero : data)}
}),
);
```

在前面的代码示例中，我们可以看到我们为更新功能添加了一个新的 reducer。它接受来自 addAntiHeroState action 的修改后的反英雄数据，并返回带有修改后的 antiHeroes 值的新状态，其中我们使用 map() 操作符用新对象替换具有给定 ID 的反英雄。

如果 reducer 成功修改了 antiHeroes 状态的值，订阅此状态更改的任何 selector 都会在组件中发出新值。

######### 派发 action

我们需要做的最后一步是向我们的组件派发 action。为了实现此步骤，我们将执行与为添加功能所做的相同的步骤。我们仍将使用 anti-hero/components/anti-hero-form.component.ts 文件来更新数据。

这里唯一的差别是我们将在表单中绑定选定的反英雄值；反英雄表单组件应接受一个反英雄对象并应使用 patchValue() 方法在表单组中修补值。

让我们来看一下下面的代码示例：

```
export class AntiHeroFormComponent implements OnInit {
    @Input() actionButtonLabel: string = 'Create';
    @Input() selectedAntiHero: AntiHero | null = null;
    @Output() action = new EventEmitter();
    form: FormGroup;
    constructor(private fb: FormBuilder) {
        this.form = this.fb.group({
            id: [''],
            firstName: [''],
```

```
lastName: [''],
    house: [''],
    knownAs: ['']
  })
}
ngOnInit(): void {
  this.checkAction();
}

checkAction() {
  if(this.selectedAntiHero) {
    this.actionButtonLabel = "Update";
    this.patchDataValues()
  }
}
emitAction() {
  this.action.emit({value: this.form.value,
    action: this.actionButtonLabel})
}
}
```

在前面的代码示例中，我们可以看到我们添加了 checkAction() 函数，该函数检查是否在反英雄表单组件中传递了反英雄对象。

这表明如果对象不为 null，这将是一个 Update action，我们必须通过使用 patchValue() 方法绑定表单，将选定的反英雄详细信息显示在每个字段中。

现在让我们看一下表单组件的代码实现：

```
// form.component.html
<app-anti-hero-form [selectedAntiHero]="antiHero"
(action)="formAction($event)"></app-anti-hero-form>

// form.component.ts
antiHero$: Observable<AntiHero | undefined>;
  antiHero: AntiHero | null = null;

  constructor(private router: ActivatedRoute,
    private store: Store<AppState>) {
    const id = this.router.snapshot.params['id'];
```

```
this.antiHero$ = this.store.select(selectAntiHero(id));
this.antiHero$.subscribe(d => {
  if(d) this.antiHero = d;
});
}
formAction(data: {value: AntiHero, action: string}) {
  switch(data.action) {
    case "Create" : {
      this.store.dispatch({type:
        AntiHeroActions.ADD_ANTI_HERO_API,
        payload: data.value});
      return;
    }
    case "Update" : {
      this.store.dispatch({type:
        AntiHeroActions.MODIFY_ANTI_HERO_API,
        payload: data.value});
      return;
    }
    default: ""
  }
}
```

在前面的代码示例中，我们可以看到我们在 formAction() 函数中添加了一个新的 case，它也派发一个 action，但类型为 MODIFY_ANTI_HERO_API。

我们还使用了 selectAntiHero() selector 来使用我们在 URL 路由中的 ID 选择反英雄，该 ID 将传递到我们的 anti-hero-form.component.ts 文件中。





## Summary

至此，本章已结束。让我们回顾一下我们学到的宝贵内容；我们已使用 NgRx 的构建块完成了应用的 CRUD 功能，并且我们学习了在状态管理中使用和不使用副作用的区别。副作用对于将 store 中的更改与数据库同步至关重要。

我们还逐步学习了如何使用应用所需的不同 action 来创建 NgRx 的构建块。

在下一章中，我们将学习如何在 Angular 中应用安全功能，例如添加用户登录和注销、检索用户配置文件信息、保护应用路由，以及调用受保护的端点 API。

## Adding Authentication in Angular

在上一章中，我们使用 NgRx 的构建块完成了 Angular 应用的 CRUD 功能。我们还学习了在应用中编写 actions、reducers 和 effects 的逐步流程，这些将用于修改状态的值。我们还学习了在应用中使用和不使用 effects 的区别。Effects 对于我们与外部 API 通信至关重要，从而使数据库更改能够在 NgRx store 中同步。

在本章中，我们将学习如何在 Angular 应用中添加身份验证；我们将实现一个登录页面，该页面将提供有效的 JWT、保护路由，并使用 NgRx 应用 API 身份验证。

在本章中，我们将涵盖以下主题：
- Adding user authentication
- Protecting routes
- Calling an API

### Technical requirements

本章的完整代码可在以下位置找到：https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-14。

### Adding user authentication

添加用户身份验证是开发应用的主要需求之一。此功能允许我们将页面和功能限制为未经授权的用户。我们可以通过不同方式实现用户身份验证，其中一种实现方式是提供登录页面以请求凭据。

让我们看一下实现身份验证功能的逐步流程。

## The authentication API

让我们首先回顾一下我们在 Spring Boot 项目中创建的身份验证 API。身份验证的端点如下：
- `{BASE_URL}/authenticate`: 身份验证的主要端点接受包含 email 和 password 字段的对象，并返回一个有效的 JWT，该 JWT 将用于调用端点。以下是该端点的示例响应对象：

```
// valid JWT
{
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJOZXN0QGDtYW1sLmNvbSIImIhdCI6MTY1OTQyODk2MSwiZXhwIjIoxNjUSNDY0OTYxfQ.WU_aZjmI fw--LCovx4cZ4_hcOTGiAgPnSaM0bjdv018"
}
```
- `{BASE_URL}/register`: 用于创建新的有效登录凭据的端点。JWT，如第 7 章 Adding Spring Boot Security with JWT 中所述，主要用于无法维护客户端状态的 RESTful Web 服务，因为 JWT 包含与用户相关联的一些信息。这将主要用于我们将请求的端点的 headers 中。

在我们的项目中，让我们在 core/services 文件夹下创建一个名为 AuthenticateService 的服务，通过执行以下命令：

```
ng g core/services/authenticate
```

成功创建服务后，我们将在服务中放置以下代码：

```
export class AuthenticateService {

  constructor(private http: HttpClient) { }
  // for login endpoint
  login(data: {email: string, password: string}): Observable<any> {
    return this.http.post<any>(
      `${environment.authURL}/authenticate`,
      data).pipe(
      tap((data: any) => data),
      catchError(err => throwError(() => err))
    )
  }
  // for register endpoint
  register(data: {email: string, password: string}):
```

```
Observable<any> {
  return this.http.post<any>(
    `${environment.authURL}/register`, data).pipe(
    tap((data: any) => data),
    catchError(err => throwError(() => err))
  )
}
```

AuthenticateService 将保存我们将在登录页面中使用的两个端点。现在，让我们为我们的应用创建拦截器。

## The HTTP interceptor

HTTP 拦截器是 Angular 的功能，允许我们拦截 HTTP 请求以转换其 headers、主体或响应。这提供了 intercept() 函数，该函数将使我们能够获取传出请求并调用下一个拦截器或后端。

我们主要使用拦截器来修改端点请求的 headers，这将负责为每个调用的请求添加 Authorization: Bearer {JWT} header。

为了实现拦截器，我们将创建 core/interceptors/header.interceptor.ts 文件，并在其中放置以下代码：

```
@Injectable()
export class HeaderInterceptor implements HttpInterceptor {
  intercept(httpRequest: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const Authorization = localStorage.getItem('token') ? `Bearer ${localStorage.getItem('token')}` : '';
    if (httpRequest.url.includes('/api/v1'))
      return next.handle(httpRequest.clone({ setHeaders: {
        Authorization }
      }));
    else
      return next.handle(httpRequest);
  }
}
```

在前面的代码示例中，我们为 intercept() 函数添加了新的实现。第一步是检索我们在本地存储中的有效 JWT，该 JWT 将用于 HTTP headers。我们仅在请求端点包含 api/v1 子字符串时使用 JWT，因为这些是受保护的端点。

下一步是克隆请求并在克隆的请求中添加 Authorization: Bearer {JWT} header，然后调用 next() 函数以使用添加的 header 调用 API。

我们现在已创建拦截器；最后一步是在 AppModule 中添加拦截器。

让我们看一下以下代码：

```
providers: [
  { provide: HTTP_INTERCEPTORS, useClass:
      HeaderInterceptor, multi: true }
],
```

在前面的代码示例中，我们现在将拦截 anti-heroes 端点上的每个 HTTP 调用，并将生成的 JWT 添加到请求 headers 中。

## The authentication module

下一步是创建身份验证模块；该模块将负责保存登录和注册页面，该页面将接受用户和凭据并调用 authenticate 和 register 端点。

要创建身份验证模块，我们将执行以下命令：

```
ng g m auth
```

成功创建身份验证模块后，我们将导入身份验证模块所需的多个模块：

```
@NgModule({
  declarations: [
  ],
  imports: [
    CommonModule,
    MaterialModule,
    FormsModule,
    ReactiveFormsModule,
    CoreModule,
  ]
});
```

现在，我们将创建模块的不同部分。

## The authentication form

我们将为身份验证模块创建主要表单；这被认为是模块的 dumb 组件，因为它将接受表单的值并将其发出到登录和注册页面。

要创建身份验证表单组件，我们将执行以下命令：

```
ng g c auth/components/auth-form
```

成功创建组件后，我们现在将实现表单的代码。在 auth-form 组件的 TypeScript 文件中，我们将放置以下代码：

```
export class AuthFormComponent {
  @Input() error: string = "";
  @Input() title: string = "Login"
  @Output() submitEmitter = new EventEmitter();
  form: FormGroup;
  constructor(private fb: FormBuilder) {
    this.form = this.fb.group({
      email: [''],
      password: ['']
    })
  }
  submit() {
    this.submitEmitter.emit(this.form.value);
  }
}
```

在前面的代码示例中，我们可以看到我们已使用 email 和 password 表单控件创建了一个响应式表单。我们还创建了一个 emitter，该 emitter 将表单的值传递到父组件，因为该组件将同时被登录和注册页面使用。现在，我们将实现 auth-form 组件的 HTML 代码和 CSS。

> **Note**
> 请参阅提供的链接以获取完整的代码实现：
> https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-14/superheroes/src/app/auth/components/auth-form





在已实现的的代码中，我们将响应式表单与电子邮件和密码输入绑定。我们还创建了一个条件，使按钮在页面当前处于登录或注册时发生变化。

我们已成功创建了我们的身份验证表单；现在，我们将创建我们的登录和注册页面。

## 登录和注册页面

登录和注册页面被视为我们应用程序的智能组件，因为这些是会分派调用身份验证 API 的 action 的组件。

要创建登录和注册页面，我们将执行以下命令：

```
ng g c auth/page/login auth/page/register
```

成功创建两个页面后，我们将运行登录和注册组件的代码：

### 登录页面

```
```typescript
//TS File
export class LoginComponent{
  constructor(private authService: AuthenticateService,
      private router: Router) {
  }
  submit(data:{email:string, password:string}) {
    this.authService.login(data).subscribe((data) => {
      this.router.navigate(['/anti-heroes']);
      localStorage.setItem('token', data.token);
    });
  }
}
// HTML File
<app-auth-form (submitEmitter)="submit($event)"></app-auth-form>
```
```

### 注册页面

```
```typescript
// TS File
export class RegisterComponent {
  error: string = "";
```
```

```
constructor(private authService: AuthenticateService) {
}
submit(data: User) {
    this.authService.register(data).subscribe((data) => {
        this.router.navigate([' /']);
    });
}
// HTML File
<app-auth-form title="Register"
(submitEmitter)="submit($event)"></app-auth-form>
```

在前面的代码示例中，我们可以看到登录页面和注册页面正在使用相同的身份验证表单组件。一旦表单提交，它将把表单值传递到 login() 或 register() 函数中，分别用于验证或创建用户。如果登录成功，我们将把用户重定向到反英雄列表页面，并将 API 生成的 token 放入本地存储中。

## 路由模块

下一步是创建 auth-routing 模块，该模块将定义身份验证模块的路由。要创建该模块，让我们执行以下命令：

```
ng g m auth/auth-routing --flat
```

创建路由模块后，我们将运行以下代码：

```
const routes: Routes = [
  {
    path: "",
    component: LoginComponent
  },
  {
    path: "register",
    component: RegisterComponent
  }
];
@NgModule({
  declarations: [],
```

```
imports: [RouterModule.forChild(routes)],
exports: [RouterModule],
})
export class AuthRoutingModule {}
```

我们还需要修改我们的 app-routing 模块，因为我们需要我们的基本路径重定向到登录页面；让我们实现以下修改：

```
const routes: Routes = [
  {
    path: "",
    redirectTo: "login",
    pathMatch: "full",
  },
  {
    path: "login",
    loadChildren: () =>
      import("./auth/auth.module").then((m) => m.AuthModule),
  },
  {
    path: "anti-heroes",
    loadChildren: () =>
      import("./anti-hero/anti-hero.module").then((m) =>
        m.AntiHeroModule),
  }
];
```

在前面已实现的代码中，我们可以看到一旦我们访问基本路径，这将现在加载 AuthModule 并将我们重定向到登录页面，如图 14.1 所示。

## Angular CRUD

![](img/7c3ddd2e5893c9b5a0220148ed886548_347_0.png)

图 14.1 – 登录页面

我们现在应该能够使用数据库中的用户登录。如果没有创建用户，我们可以使用注册页面创建一个新的用户，并且一旦登录成功，我们将被重定向到反英雄列表页面，如 *图 14.2* 所示。

## Angular CRUD

![](img/7c3ddd2e5893c9b5a0220148ed886548_347_1.png)

图 14.2 – 反英雄列表页面

我们还可以观察到我们的有效 JWT 已经放置在我们的本地存储中，因为 HTTP 拦截器正在使用 JWT。当我们打开由我们的应用程序发出的请求时，我们可以看到标头中具有生成的 JWT：

```
Accept:
application/json, text/plain, */*
Accept-Encoding:
gzip, deflate, br
Accept-Language:
en-AU,en-US;q=0.9,en;q=0.8,bs;q=0.7,fr-CA;q=0.6,fr;q=0.5,tl;q=0.4
Authorization:
Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0OQGdtYWlsLmNvbSIIm1hdCI6MTY1OTY5ODM2NSwiZXhwIjoxNjU5NzM0MzY1fQ.2SDLmvQcME5Be9Xj-zTeRlc6kGfQVNCMIWUBOBS5afg
```

在前面的示例标头中，我们可以看到 Authorization 标头包含应用程序调用的每个 API 请求的有效 JWT。JWT 在标头中的放置是在我们的登录成功并且我们被重定向到 AntiHeroModule 时完成的。

## Token 验证

我们需要做的下一步是添加 token 验证以检查我们的 token 是否已过期。要实现此功能，我们通过执行以下命令添加 @auth0/angular-jwt 库：

```
npm install @auth0/angular-jwt --save
```

@auth0/angular-jwt 库提供了有用的函数，例如 isTokenExpired()，用于检查 JWT 是否已过期，以及 decodeToken()，用于从 JWT 中检索信息。

成功安装库后，我们将向我们的 authenticate 服务添加以下代码：

```
isAuthenticated(): boolean {
  const token = localStorage.getItem('token') ?? '';
  // Check whether the token is expired and return
  // true or false
  return !this.jwtHelper.isTokenExpired(token);
}
```

我们还需要将 JWT 模块导入到我们的 `app.module.ts` 文件中：

```
```typescript
imports: [
    JwtModule.forRoot({})
],
```
```

我们将在我们的登录和注册页面上使用 `isAuthenticated()` 函数来检查本地存储中是否存在 JWT。如果存在有效的 JWT，我们将把应用程序重定向到反英雄列表页面。

让我们看看以下实现：

```
```typescript
//login page (TS File)
constructor(private authService: AuthenticateService, private
router: Router,) {
    this.checkJWT();
}
checkJWT() {
    if(this.authService.isAuthenticated()) {
        this.router.navigate(['/anti-heroes'])
    }
}
```
```

## 注销实现

我们需要实现的最后一个功能是注销功能。要添加此功能，我们唯一需要添加的就是一个将 token 从存储中移除的函数。让我们按如下方式进行代码实现：

### authenticate.service.ts

```
```typescript
export class AuthenticateService {
... other functions
doLogout() {
    let removeToken = localStorage.removeItem('token');
    if (removeToken == null) {
        this.router.navigate(['login']);
    }
}
}
```
```

在前面的代码示例中，我们添加了一个 doLogout() 函数，该函数移除存储中的 token 并将应用程序重定向到登录页面。现在，让我们编辑我们的 navbar 组件以具有注销按钮：

### navbar.component.html

```
<p>
    <mat-toolbar color="primary">
        <span>Angular CRUD</span>
        <span class="example-spacer"></span>
        <button *ngIf="loggedIn" (click)="submit('logout')" mat-icon-button>
            <mat-icon>logout</mat-icon>
        </button>
    </mat-toolbar>
</p>
```

### navbar.component.css

```
.example-spacer {
    flex: 1 1 auto;
}
```

### navbar.component.ts

```
export class NavbarComponent implements OnInit{
    @Output() actionEmitter = new EventEmitter();
    @Input() loggedIn = false;
    submit(action: string) {
        this.actionEmitter.emit(action);
    }
}
```

在前面的代码实现中，我们为我们的 navbar 组件创建了一个 emitter。这将发出我们在 navbar 中触发的 action，并将其传递到我们的 app 组件中。

最后一步是在注销按钮被点击时在我们的 app 组件中调用 doLogout() 函数。让我们看看如下的代码实现：

### app.component.html

```
```html
<app-navbar [loggedIn]="url != '/' && !url.includes('login')"
(actionEmitter)="submit($event)"></app-navbar>
<div class="container">
    <router-outlet></router-outlet>
</div>
```
```

### app.component.ts

```
```typescript
export class AppComponent {
  title = 'superheroes';
  url: string = "";
  constructor(private authService: AuthenticateService,
              private router: Router){
    this.getRoute();
  }
  submit(action: string) {
    switch (action) {
      case 'logout':
        this.authService.doLogout();
        break;
      default:
        break;
    }
  }
  getRoute() {
    this.router.events.subscribe(data => {
      if(data instanceof NavigationEnd) {
        this.url = data.url;
      }
    });
  }
}
```



在前面的代码实现中，我们将 authenticate 服务注入到我们的应用组件中并调用了 doLogout() 函数。如果操作是注销，我们还添加了一个路由变化的监听器来检查我们当前的路由是否在登录或注册页面，如果是，我们将移除导航栏组件上的注销按钮。

我们已经成功为我们的应用实现了用户认证，但在本章的后续内容中我们仍会继续改进此实现。在下一节中，我们将讨论如何在 Angular 应用中保护路由。

## 保护路由

Angular 的核心特性之一是路由守卫。守卫在我们想要防止未认证直接访问路由或防止用户在意外导航时丢失更改时非常有帮助。

守卫是 Angular 提供的接口，允许我们通过提供的条件来控制路由的可访问性。这些守卫直接应用于我们想要保护的路由。

让我们来看一下 Angular 提供的一些守卫：

- CanActivate：这是在我们想要阻止访问的路由上实现的。
- 方法签名：

```
canActivate(route:ActivatedRouteSnapshot,
state:RouterStateSnapshot):Observable<boolean | UrlTree>
| Promise<boolean | UrlTree> | boolean | UrlTree
```

前面的代码定义了 CanActivate 守卫的签名。该函数接受 ActivatedRouteSnapshot 和 RouterStateSnapshot 参数，并返回一个 Observable 或 Promise，其类型可以是 Boolean 或 UrlTree。

- 创建守卫：

```
export class AuthGuard implements CanActivate {
constructor(private auth: AuthService, private router: Router) {}
canActivate(route: ActivatedRouteSnapshot,
state:RouterStateSnapshot): Observable<boolean> |
Promise<boolean> | boolean {
// return true permitted in the route, else return
// false
}
}
```

在前面的代码示例中，我们创建了一个名为 AuthGuard 的新类。我们还使用 CanActivateChild 守卫实现了它，并为所需逻辑添加了 canActivateChild() 函数。

- 使用守卫：

```
{
  path: 'user',

  canActivateChild: [AuthGuard],
  component: UserComponent,
  children: [
    { path: ':id', component: ProfileComponent },
    { path: ':id/edit', component: SettingsComponent }
  ]
}
```

在前面的代码示例中，我们在 user 路径中使用了新创建的 AuthGuard 类，以保护其子路由免受没有有效 JWT 的用户访问，子路由导航到 ProfileComponent 和 SettingsComponent 组件。

- CanLoad：此守卫用于懒加载模块。CanActivate 守卫只能阻止用户通过路由导航；CanLoad 守卫可同时阻止导航到懒加载模块以及下载该模块。

- 方法签名：

```
canLoad(route:Route,segments:UrlSegment[]):
Observable<boolean>|Promise<boolean>|boolean
```

前面的代码示例定义了 CanLoad 守卫的签名。该函数接受 Route 和 UrlSegment[] 参数，并返回一个 Observable 或 Promise，其类型可以是 Boolean。

- 创建守卫：

```
import { CanLoad, Route, Router } from '@angular/router';
export class AuthGuard implements CanLoad {
  constructor(private router: Router) {}
  canLoad(route:Route,segments:UrlSegment[]):Observable<boolean>|Promise<boolean>|boolean {
    // return true or false based on a condition to load
    // a module or not
  }
}
```

在前面的代码示例中，我们创建了一个名为 AuthGuard 的新类。我们还使用 CanLoad 守卫实现了它，并为所需逻辑添加了 canLoad() 函数。

使用守卫：

```
{
  path: "hero",
  loadChildren: () =>
    import("./hero/hero.module").then((m) =>
      m.AntiHeroModule),
  canLoad: [AuthGuard]
}
```

在前面的代码示例中，我们在 hero 路由中使用了新创建的 AuthGuard 类，以防止没有有效 JWT 的用户访问和下载资源。

CanDeactivate：这是一个用于防止用户离开当前路由的守卫。这在填写应用中的表单等场景下很有用，以避免在意外导航时丢失更改。

方法签名：

```
canDeactivate(component: T, currentRoute: ActivatedRouteSnapshot, currentState: RouterStateSnapshot, nextState?: RouterStateSnapshot): Observable<boolean|UrlTree>|Promise<boolean|UrlTree>|boolean |UrlTree;
```

前面的代码示例定义了 CanDeactivate 守卫的签名。该函数接受一个泛型组件、ActivatedRouteSnapshot 和 RouterStateSnapshot 参数，并返回一个 Observable 或 Promise，其类型可以是 Boolean 或 UrlTree。

创建守卫：

```
// CanDeactivateGuard service
import { Observable } from 'rxjs/Observable';
import { CanDeactivate, ActivatedRouteSnapshot,
RouterStateSnapshot } from '@angular/router';
export interface CanComponentDeactivate {
canDeactivate: () => Observable<boolean> |
Promise<boolean> | boolean;
}
```

```
export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
    canDeactivate(component:CanComponentDeactivate,currentRoute:ActivatedRouteSnapshot, currentState:RouterStateSnapshot, nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
        return component.canDeactivate();
    }
}
```

在前面的实现中，我们创建了一个接口，该接口将在 CanDeactivateGuard 服务的组件中使用：

```
export class FormComponent implements OnInit, CanComponentDeactivate {

canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
/* return true or false depends on a specific condition if you want to navigate away from this route or not.*/
}
}
```

在前面的代码示例中，我们为组件实现了我们创建的接口。

- 使用守卫：

```
{ path: ':id/edit', component: FormComponent, canDeactivate: [CanDeactivateGuard] }
```

在前面的代码示例中，我们使用了新创建的 CanDeactivateGuard 来防止用户根据 canDeactivate() 函数上的应用条件离开 FormComponent。

我们已经了解了可以在应用中使用的不同守卫。现在，让我们在 Angular 项目中实现它。

## 项目实现

我们需要在应用中应用的第一个守卫是 CanLoad 守卫。这是因为我们希望在没有有效 JWT 时保护 anti-heroes 路由不被访问。要创建 CanLoad 守卫，执行以下命令：

```
ng g g core/guards/auth
```

执行命令后，选择 CanLoad 选项以生成新的 AuthGuard 类。这里唯一需要更改的是 canLoad() 函数的实现。我们想要应用的条件是：如果 JWT 有效，则允许加载路由和模块。

让我们来看一下以下实现：

```
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanLoad {
  constructor(private router: Router, private auth: AuthenticateService) {}
  canLoad(route: Route, segments:UrlSegment[]):
    Observable<boolean | UrlTree> | Promise<boolean | UrlTree> | boolean | UrlTree {
    if (!this.auth.isAuthenticated()) {
      this.router.navigate(['login']);
      return false;
    }
    return true;
  }
}
```

在前面的代码示例中，我们使用了 isAuthenticated() 函数来检查 JWT 是否有效且未过期。如果有效，它将返回 true 并允许我们导航路由。否则，它将重定向我们到登录页面。

最后一步是在 anti-heroes 路由中应用 AuthGuard 类；在 app-routing.module.ts 文件中，我们将使用以下内容：

```
const routes: Routes = [
  ... other routes here
  {
    path: "anti-heroes",
    loadChildren: () =>
      import("./anti-hero/anti-hero.module").then(m => m.AntiHeroModule),
    canLoad: [AuthGuard]
  }
];
```





我们现在已成功在我们的反英雄路由中应用了 CanLoad 守卫。为了测试它是否有效，我们可以尝试删除本地存储中的令牌，这样应该会将我们重定向到登录页面，因为没有有效的 JWT。

我们需要的最后一个路由守卫是 CanDeactivate 守卫；我们将把此守卫应用到我们的反英雄表单上，以防止用户在离开表单时丢失更改。要创建我们的 CanDeactivate 守卫，我们将执行以下命令：

```
ng g g core/guards/form
```

执行命令后，选择 CanDeactivate 选项，这将生成一个新的 FormGuard 类。我们将向此类添加一个接口，我们将在表单组件中使用它。

让我们看一下以下代码：

```
export interface CanComponentDeactivate {
  canDeactivate: () => Observable<boolean> |
    Promise<boolean> | boolean;
}
@Injectable({
  providedIn: 'root'
})
export class FormGuard implements CanDeactivate<unknown> {
  canDeactivate(
    component: CanComponentDeactivate,
    currentRoute: ActivatedRouteSnapshot,
    currentState: RouterStateSnapshot,
    nextState?: RouterStateSnapshot): Observable<boolean |
      UrlTree> | Promise<boolean | UrlTree> | boolean |
      UrlTree {
    return component.canDeactivate ?
      component.canDeactivate() : true;
  }
}
```

在前面的代码示例中，我们创建了 CanComponentDeactivate 接口，表单组件将实现它。这意味着条件将放在组件中而不是守卫中。在 FormComponent 中，我们将添加以下代码：

```
export class FormComponent implements OnInit,
CanComponentDeactivate {
```

```
... other code implementation
canDeactivate(): Observable<boolean> | Promise<boolean> |
  boolean {
  const confirmation = window.confirm('Are you sure?');
  return confirmation;
}
  ... other code implementation
}
```

在前面的代码示例中，我们使用我们创建的 CanComponentDeactivate 接口实现了 FormComponent；我们添加了一个 window.confirm()，它会弹出一个对话框，询问用户是否要离开当前路由。这是对守卫的简单实现，因为我们也可以添加其他条件，例如如果表单中有更改时才询问这个问题。

最后一步是在 FormComponent 路由中应用守卫。

让我们看一下以下代码：

```
const routes: Routes = [
  ... other routes
  {
    path: "form",
    children: [
      {
        path: "",
        canDeactivate: [FormGuard],
        component: FormComponent
      },
      {
        path: ":id",
        canDeactivate: [FormGuard],
        component: FormComponent
      }
    ]
  },
];
```

一旦我们应用了 CanDeactivate 守卫，从反英雄表单导航出去时将弹出一个对话框供用户确认，如图 14.3 所示。

![](img/7c3ddd2e5893c9b5a0220148ed886548_360_0.png)

14.3 – 离开表单时弹出的对话框

我们现在已成功在我们的 Angular 应用中应用了守卫；在下一节中，我们将直接使用 NgRx 状态管理来改进我们对 API 认证的调用。

## 调用 API

我们在上一节中已经通过直接在组件中调用认证服务创建了用户认证。我们还使用 setItem 函数将生成的 JWT 存储在本地存储中，这也在我们的登录组件中发生。

我们想要实现的是减少组件的职责，正如我们记得的，我们正在使用 NgRx 状态管理来调用 API，组件的唯一职责是分发 action，而 NgRx 将完成其余工作。

在本节中，我们将通过使用 NgRx 状态管理的构建块来改进我们的 API 调用。

## 创建 actions

我们需要的第一步是为我们的认证功能创建 actions。我们将在 auth/state 文件夹中创建一个名为 auth.actions.ts 的文件，并将有以下代码：

```
import { createAction, props } from '@ngrx/store';

export enum AuthActions {
  LOGIN = '[AUTH] Login',
  SET_TOKEN = '[AUTH] Set Token',
  CREATE_USER = '[AUTH] Create User',
  AUTH_ERROR = '[AUTH] AUTH_ERROR',
}
export const setToken = createAction(
  AuthActions.SET_TOKEN,
  props<{ token: string }>(),
);
export const setError = createAction(
  AuthActions.AUTH_ERROR,
  props<{ error: any }>(),
);
```

在前面的代码中，我们可以看到我们创建了四种 action 类型：LOGIN 类型将用于负责调用登录 API 的 effect；CREATE_USER 类型将用于负责调用注册 API 的 effect；SET_TOKEN 类型将由 reducer 使用，在登录 API 成功后将生成的 JWT 设置到 store 中；最后，AUTH_ERROR 类型将用于在登录或注册 API 返回错误时在 store 中设置错误。

## 创建 effects

创建 actions 后，现在我们将创建用于调用登录和注册 API 的 effects。我们将在 auth/state 文件夹中创建一个名为 auth.effects.ts 的文件，并将有以下实现：

### Login Effect

```
@Injectable()
  loginUser$ = createEffect(() => {
    return this.actions$.pipe(
      ofType(AuthActions.LOGIN),
```

```
mergeMap(( (data: {type: string, payload: User}) =>
    this.authService.login(data.payload)
    .pipe(
        map(data => ({ type: AuthActions.SET_TOKEN,
            token: data.token })),
        tap(() =>
            this.router.navigate(["anti-heroes"])),
        catchError(async (data) => ({ type:
            AuthActions.AUTH_ERROR, error: data.error }))
    ))
}, {dispatch: true}
```

### Register Effect

```
createUser$ = createEffect(() => {
    return this.actions$.pipe(
        ofType(AuthActions.CREATE_USER),
        mergeMap(( (data: {type: string, payload: User}) =>
            this.authService.register(data.payload)
            .pipe(
                tap(() => this.router.navigate(["login"])),
                catchError(async (data) => ({ type:
                    AuthActions.AUTH_ERROR, error: data.error }))
            )
        )
    }, {dispatch: true}
);
```

在前面的代码中，我们为登录和注册 API 创建了 effects。在 loginUser$ effect 中，一旦登录 API 成功，它将分发 SET_TOKEN action 并传递生成的 JWT，并且这也将把我们重定向到反英雄页面。

这是我们在上一节中实现的行为。另一方面，createUser$ effect 一旦注册 API 成功，将再次把我们重定向到登录页面。这是一个简单的行为，你可以根据注册成功后需要发生的情况进行自定义。

我们还实现了 AUTH_ERROR action，当登录或注册 API 失败时将被调用。

## 创建 reducers

我们需要的下一步是创建 reducers。我们将在 auth/state 文件夹中创建一个名为 auth.reducers.ts 的文件，并将有以下实现：

```
export interface AuthState {
    token: string;
    error: any
}
export const initialState: AuthState = {
    token: "",
    error: null
}
export const authReducer = createReducer(
    initialState,
    on(setToken, (state, { token }) => { return {...state, token}}),
    on(setError, (state, { error }) => { return {...state, error}}),
);
```

在前面的代码示例中，我们可以看到 AuthState 有两个字段，即 token 和 error。token 字段将在 setToken action 被调用时包含有效的 JWT，即认证 API 成功时，而 error 字段将在登录或注册 API 失败时包含生成的错误。

## 创建 selectors

创建 reducers 后，我们现在将创建我们的 selector。在这种情况下，我们的 selector 很简单，因为我们只需要一个用于 error 字段的 selector。我们将在 auth/state 文件夹中创建一个名为 auth.selectors.ts 的文件，并将有以下实现：

```
import { createSelector, createFeatureSelector } from '@ngrx/store';
import { AppState } from 'src/app/state/app.state';
import { AuthState } from './auth.reducers';
export const selectAuthState = createFeatureSelector<AuthState>('authState')
export const selectError = () => createSelector(


```
selectAuthState,
    (state: AuthState) => state.error
)
```

在前面的代码示例中，我们为 error 字段创建了一个选择器；我们将需要此选择器在组件中向用户显示错误信息。

## 在本地存储中同步

我们将要实现的下一个功能是在本地存储中同步我们的状态。我们可以通过在应用程序中使用 localStorage.setItem() 来实现这一点。然而，这样做将不可维护，并且存储中值的设置会在不同的地方进行。

为了获得更好的实现，我们将使用 ngrx-store-localstorage 库。要安装该库，我们将执行以下命令：

```
npm install ngrx-store-localstorage --save
```

成功安装库后，我们应该确定要与本地存储同步的状态。在我们的例子中，我们希望 auth 状态中的 token 字段得到同步。为此，我们在 auth.module.ts 中进行以下代码更改：

```
import { localStorageSync } from 'ngrx-store-localstorage';
export function localStorageSyncReducer(reducer: ActionReducer<any>): ActionReducer<any> {
  return localStorageSync({keys: ['token']})(reducer);
}
const metaReducers: Array<MetaReducer<any, any>> = [localStorageSyncReducer];

@NgModule({
  declarations: [
    ... declared components
  ],
  imports: [
    ... other imported modules
    StoreModule.forFeature('authState', authReducer, {metaReducers}),
    EffectsModule.forFeature([AuthEffects]),
  ]
})
```

在前面的代码中，我们可以看到我们创建了一个专门的 reducer，它调用来自 ngrx-store-localstorage 的 localStorageSync，负责在本地存储中添加值。

我们还可以指定要同步的字段，在本例中，我们在 keys 数组中添加了 token。一旦 token 状态的值发生变化，新值也将被放置在我们的存储中。

## 分发和选择组件

最后一步是分发 action 并为登录和注册组件使用选择器。让我们看一下登录和注册组件的以下代码实现：

### login.component.ts

```
export class LoginComponent{
  error$ = this.store.select(selectError());
  errorSub: Subscription | undefined;

  constructor(private store: Store, private authService:
    AuthenticateService, private router: Router,
    private _snackBar: MatSnackBar) {
    this.checkJWT();
    this.getError();
  }
  submit(data: User) {
    this.store.dispatch({type: AuthActions.LOGIN,
      payload: data})
  }
  ngOnDestroy(): void {
    this.errorSub?.unsubscribe();
  }
  getError() {
    this.error$.subscribe(data => {
      if(data) {
        this._snackBar.open(data.message, "Error");
      }
    })
  }
```

```
}
... other code implementation
}
```

### register.component.ts

```
export class RegisterComponent implements OnInit, OnDestroy {
  error$ = this.store.select(selectError());
  errorSub: Subscription | undefined;
  constructor(private store: Store, private _snackBar:
      MatSnackBar) {
    this.getError();
  }

  ngOnDestroy(): void {
    this.errorSub?.unsubscribe();
  }
  submit(data: User) {
    this.store.dispatch({type: AuthActions.CREATE_USER,
      payload: data})
  }
  getError() {
    this.errorSub = this.error$.subscribe(data => {
      if(data) {
        this._snackBar.open(data.message, "Error");
      }
    })
  }
  ... other code implementation
}
```

我们可以在前面的代码中看到，登录和注册页面几乎具有相同的实现。我们已经移除了 submit 函数中对登录和注册服务的调用，并用 action 的分发替换了它。我们还使用了 selectError() 选择器来监听 API 是否产生错误。

## 总结

至此，我们已到达本章的结尾；让我们回顾一下你学到的宝贵内容。

我们现在知道如何在 Angular 应用程序中实现用户身份验证，并且我们使用了一个 HTTP 拦截器来拦截 HTTP 请求以转换其头并为 API 调用添加有效的 JWT。我们还了解了不同的路由守卫，它们允许我们保护路由免受未经授权的访问，或防止在从路由导航离开时意外丢失数据。最后，我们学习了如何通过改进 Angular 中身份验证的实现方式来使用 NgRx 状态管理。

下一章将教我们如何使用 Cypress 框架在 Angular 中编写端到端测试。
