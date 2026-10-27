# Chapter 6. Dependency Injection

The idea behind dependency injection is very simple. If you have a component that depends on a service, you do not create that service yourself. Instead, you request one in the constructor, and the framework will provide you one. By doing so you can depend on interfaces rather than concrete types. This leads to more decoupled code, which enables testability, and other great things.

![](bbox=[0.15, 0.33, 0.85, 0.58])

Angular comes with a dependency injection system. To see how it can be used, let's look at the following component, which renders a list of talks using the for directive:

```
@Component({
  selector: 'talks-cmp',
  template: `
    <h2>Talk s:</h2>
    <talk *ngFor="let t of talks" [talk]="t"></talk>
  `
})
class TalksCmp {
  constructor() { //..get the data }
}
```