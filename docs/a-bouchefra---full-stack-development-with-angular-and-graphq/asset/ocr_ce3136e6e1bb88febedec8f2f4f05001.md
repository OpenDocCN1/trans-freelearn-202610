9. Next, let's add a Material card that contains the HTML form; open the src/app/users/signup/signup.component.html file and update it, as follows:

```
<mat-card>
  <mat-card-title>Create a new account</mat-card-title>
  <mat-card-subtitle>It's quick and easy.</mat-card-subtitle>
  <mat-card-content>
    <!-- ADD_FORM_HERE -->
  </mat-card-content>
</mat-card>
```

10. Add the first section of the form, as follows:

```
<form [formGroup]="form" (ngSubmit)="submit()">
  <mat-form-field>
    <mat-icon matPrefix>person</mat-icon>
    <input autofocus type="text" matInput
      placeholder="Name *" formControlName="fullName">
    <mat-hint align="start">
      Enter your full name.
    </mat-hint>
  </mat-form-field>
</form>
```

We use the formGroup property to bind our form to the form group we created in the component's class, and we also bind the ngSubmit event of the form to the submit() method, which will be invoked when we submit the form. We use the <mat-form-field> component to wrap the form's fields to provide common Material styles and behavior to form controls such as the floating label, underlines, and hint messages. We use the <mat-icon> component with the matPrefix directive to display an icon in front of the form's field.

Next, we use the native <input> element with the matInput directive to allow the element to work with <mat-form-field>. We use the autofocus attribute to automatically give focus to the input element and formControlName to bind the element to the fullName property of the corresponding form's group defined in the component's class.