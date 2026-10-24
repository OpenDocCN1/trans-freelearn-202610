The preceding code contains three data-cy attributes, which you can find in the td element, and two button elements.

8. Next, we must edit the anti-hero-command-bar.component.html file using the following code:

```
<p>
  <mat-toolbar>
    <button mat-raised-button color="primary"
      (click)="emitAction(0)" data-cy="create">
      <mat-icon>fiber_new</mat-icon> Create
    </button>
    &nbsp;
    <button mat-raised-button color="warn"
      (click)="emitAction(1)" data-cy="delete-all">
      <mat-icon>delete</mat-icon> Delete All
    </button>
    <button mat-button color="danger"
      (click)="logOut()" data-cy="logout">
      <mat-icon>logout</mat-icon> logout
    </button>
  </mat-toolbar>
</p>
```

The preceding code contains three selectors, which you can find in the button elements.

9. The last file to update is anti-hero-form.component.html:

```
<mat-card class="form-card">
  <h2>{{ selectedAntiHero ? "Update/View Hero" : "Create Hero" }}</h2>
  <form class="anti-hero-form" [formGroup]="form">
    <mat-form-field class="example-full-width" appearance="fill"><mat-label>
      First Name</mat-label>
      <input matInput formControlName="firstName" placeholder="Ex. Tony" data-cy="firstName"/>
    </mat-form-field><mat-form-field class="example-full-width" appearance="fill">
      <mat-label>Last Name</mat-label>
```