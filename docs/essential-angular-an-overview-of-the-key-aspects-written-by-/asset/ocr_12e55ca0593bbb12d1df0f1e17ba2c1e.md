# # ReactiveFormsModule

![](bbox=[0.133, 0.159, 0.869, 0.417])

`ReactiveFormsModule` is **simpler to understand and explain than FormsModule.** That's why I will cover it first.

```
```typescript
@Component({
  selector: 'filters-cmp',
  template:
    `<div [formGroup]="filters">
      <input formControlName="title" placeholder="Title">
      <input formControlName="speaker" placeholder="Speaker">
      <input type="checkbox" formControlName="highRating">
      High Rating
    </div>`
})
export class FiltersCmp {
  filters = new FormGroup({
    speaker: new FormControl(),
    title: new FormControl(),
    highRating: new FormControl(false),
  });

  constructor(app: App) {
    this.filters.valueChanges.debounceTime(200).
      subscribe((value) => {
```
```