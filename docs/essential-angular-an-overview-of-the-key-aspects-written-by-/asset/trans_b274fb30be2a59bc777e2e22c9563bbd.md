

```
  "rating": 8.0
},
...
```

## Protractor 测试

最后，我们始终可以编写一个 Protractor 测试来驱动整个应用程序。

```typescript
import {browser, element, by} from 'protractor';

export class TalksAppPage {
    navigateTo() {
        return browser.get('/');
    }

    getTitleInput() {
        return element(by.css('input[formcontrolname=title]'));
    }

    getTalks() {
        return element.all(by.css('talk-cmp'));
    }

    getTalkText(index: number) {
        return this.getTalks().get(index).getText();
    }
}

describe('e2e tests', function() {
    let page: TalksAppPage;

    beforeEach(() => {
        page = new TalksAppPage();
    });

    it('should filter talks by title', () => {
        page.navigateTo();

        const title = page.getTitleInput();
        title.sendKeys("Are we there");

        expect(page.getTalks().count()).toEqual(1);
        expect(page.getTalkText(0)).toContain("Are we there yet?");
    });
});
```

## 让我们回顾一下

在本章中，我们研究了测试 Angular 组件的四种方式：隔离测试、浅层测试、集成测试和 Protractor 测试。它们各有其适用的时机与场景：隔离测试是以测试驱动的方式开发组件、测试复杂逻辑的绝佳方法。浅层测试是隔离测试的增强版，在编写有意义的测试需要渲染组件模板时，应当使用它们。最后，集成测试和 Protractor 测试则用于验证一组组件和服务（即应用程序）能否协同工作。

## 第 10 章 Angular 中的响应式编程

## 核心框架中的响应式编程

Angular 应用程序是一个响应式系统。用户点击按钮，应用程序响应该事件并更新模型。模型更新后，应用程序会通过组件树传播这些变更。

![](img/79f67e6cb2f5bd357461768fccf131e0_81_0.png)

Angular 以截然不同的方式实现了这两个箭头。让我们来探讨一下原因。

## 事件与状态

要理解为什么 Angular 使用两种截然不同的响应式编程方式，我们需要考察事件与状态之间的差异。

在讨论响应式编程时，我们经常会谈到事件或事件流。事件流是响应式对象的一个重要类别，状态也是如此。因此，让我们

## Angular 生态系统中的响应式编程

我们已经了解了 Angular 核心框架本身如何支持响应式编程。现在，让我们看看 Angular 生态系统。

### @angular/forms

Angular 一直对构建动态表单提供了强有力的支持。这也是该框架如此成功的主要原因之一。

现在，框架附带了一个模块，用于通过具体化响应式编程来处理输入。

```typescript
import {ReactiveFormsModule, FormGroup, FormControl} from '@angular/forms';

@Component({
  selector: 'filters-and-talks',
  template:
    `<form [formGroup]="filtersForm">
      Title <input formControlName="title">
      Speaker <input formControlName="speaker">
    </form>

    <talk *ngFor="let t of talks|async" [talk]="t"></talk>`
})
class TalksAndFiltersCmp {
  filtersForm = new FormGroup({
    title: new FormControl(''),
    speaker: new FormControl('')
  });

  talks: Observable<Talk[]>;

  constructor(backend: Backend) {
    this.talks = this.filtersForm.valueChanges.
      debounceTime(100).
      switchMap(filters => backend.fetch(filters));
  }
}

@NgModule({
  declarations: [TalksAndFiltersCmp,...
```

## 小结

Angular 应用程序是一个响应式系统。这就是为什么我们需要理解响应式编程，才能在 Angular 中高效工作。

响应式编程既处理事件流，也处理状态。它可以分为透明式和具体化式两种。

从一开始，Angular 框架就对透明式响应式编程提供了极好的支持。它既用于传播状态，也用于处理事件。它简单而快速，而且新版本的框架仍然支持它。

但它在某些时候也可能带来限制，使某些问题的解决变得困难。这就是为什么 Angular 现在通过 Observable 提供了对具体化响应式编程的支持。

Angular 生态系统也采纳了这些理念。响应式表单模块、路由器，以及其他库（如 NgRx），都提供了基于 Observable 的 API。

