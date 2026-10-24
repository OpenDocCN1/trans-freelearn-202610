```
</p>
<p>
    <mat-form-field>
        <input type="password"
            matInput placeholder="Password"
            formControlName="password"
            data-cy="password">
    </mat-form-field>
</p>
<div class="button">
    <button type="submit"
        mat-button data-cy="submit-login">
        {{title}}</button>
</div>
<p *ngIf="title == 'Login'" class="link"
    [routerLink]="['register']"
    routerLinkActive="router-link-active">
    Create account</p>
<p *ngIf="title == 'Register'"
    class="link" [routerLink]="['']"
    routerLinkActive="router-link-active">
    Sign In</p>
</form>
</mat-card-content>
</mat-card>
```

The preceding code contains four data-cy attributes that will be used as targeted selectors. You can find the data-cy selectors in the mat-card-title, inputs, and button elements.

6. The next file to update will be navbar.component.html. Use the following code to update the file:

```
<p>
    <mat-toolbar color="primary">
        <span data-cy="logo">Angular CRUD</span>
    </mat-toolbar>
</p>
```

The preceding code contains one data-cy attribute, which you can find in the span element.