# # Reactive programming in the Angular ecosystem

We have looked at how the Angular core framework itself supports reactive programming. Now let's look at the Angular ecosystem.

# ## @angular/forms

Angular has always had strong support for building dynamic forms. It's one of the main reasons the framework got so successful.

Now the framework comes with a module that adds support for handling input using reified reactive programming.

```
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
      debounceTime(100);
  }
}
```
```