# 第 9 章 测试

Angular 的设计目标之一是让测试变得容易。因此，该框架依赖依赖注入，将用户代码与框架代码分离，并提供一套用于编写和运行测试的工具。在本章中，我将介绍测试 Angular 组件的四种方式：隔离测试、浅层测试、集成测试和 Protractor 测试。

## 隔离测试

在不渲染组件的情况下测试复杂组件通常很有用。为了了解如何实现，让我们为以下组件编写一个测试：

```typescript
@Component({
  selector: 'filters-cmp',
  templateUrl: './filters.component.html',
  styleUrls: ['./filters.component.css']
})
export class FiltersCmp {
  @Output() change = new EventEmitter();

  filters = new FormGroup({
    speaker: new FormControl(),
    title: new FormControl(),
    highRating: new FormControl(false),
  });

  constructor(@Inject('createFiltersObject') createFilters: Fur
    this.filters.valueChanges.debounceTime(200).\
    subscribe((value) => {
      this.change.next(createFilters(value));
    });
  }
}
```

下面是 `filters.component.html` 的代码：

```html
<div [formGroup]="filters">
  <md-input-container>
    <input md-input formControlName="title" placeholder="Title">
  </md-input-container>

  <md-input-container>
    <input md-input formControlName="speaker" placeholder="Spea
  </md-input-container>

  <md-checkbox formControlName="highRating">
    High Rating
  </md-checkbox>
</div>
```

这个例子中有几点值得注意：

- 我们正在...

## 浅层测试

在不渲染模板的情况下测试组件类在某些场景下是可行的，但并非所有场景都如此。有时，我们只有渲染组件的模板才能编写有意义的测试。我们可以做到这一点，同时仍然保持测试的隔离性。我们只需要渲染模板，而不渲染组件的子组件。这就是通常所说的**浅层测试**。

让我们看看这种方法的实际应用。

```
@Component({
  selector: 'talks-cmp',
  template: '<talk-cmp *ngFor="let t of talks" [talk]="t"></ta:'
})
export class TalksCmp {
  @Input() talks: Talk[];
}
```

这个组件只是渲染一组 TalkCmp。

现在让我们看看它的测试。

```
import { async, ComponentFixture, TestBed } from '@angular/core'
import { By } from '@angular/platform-browser';
import { DebugElement, NO_ERRORS_SCHEMA } from '@angular/core';

import { TalksCmp } from './talks.component';

describe('TalksCmp', () => {
  let component: TalksCmp;
  let fixture: ComponentFixture<TalksCmp>;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [TalksCmp],
      schemas: [NO_ERRORS_SCHEMA]
    })
  })
  ...
```

## 集成测试

我们还可以编写一个集成测试，对整个应用程序进行测试。

```
import {TestBed, async, ComponentFixture, inject} from '@angular
import {AppCmp} from './app.component';
import {AppModule} from './app.module';
import {App} from "./app";

describe('AppCmp', () => {
    let component: AppCmp;
    let fixture: ComponentFixture<AppCmp>;
    let el: Element;

    beforeEach(async(() => {
        TestBed.configureTestingModule({
            imports: [AppModule]
        });
        TestBed.compileComponents();
    }));

    beforeEach(() => {
        fixture = TestBed.createComponent(AppCmp);
        component = fixture.componentInstance;
        fixture.detectChanges();
        el = fixture.debugElement.nativeElement;
    });

    it('should filter talks by title', async(inject([App], \
        (app: App) => {
        app.model.talks = [
            {
                "id": 1,
                "title": "Are we there yet?",
                "speaker": "Rich Hickey",
                "yourRating": null,
                "rating": 9.0
            },
            {
                "id": 2,
                "title": "The Value of Values",
                "speaker": "Rich Hickey",
                "yourRating": null,
```





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
