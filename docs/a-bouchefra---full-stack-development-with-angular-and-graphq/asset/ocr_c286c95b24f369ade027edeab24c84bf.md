15. Then, open the signup/signup.component.css file and start by adding the following styles for the component's host element:

```
```css
:host {
  display: flex;
  justify-content: center;
  margin: 100px 0px;
}
```
```

We set the display to flex, justify the contents to the center, and finally, set the top and bottom margins to 100 pixels and the left and right margins to 0 pixels.

16. Style the card containing the form, as follows:

```
```css
mat-card-title,
mat-card-subtitle,
mat-card-content {
  display: flex;
  justify-content: center;
}
```
```

We set the display to flex and center the contents of the card's title, subtitle, and body.

17. Style the form fields, the button, and the error message, as follows:

```
```css
.mat-form-field {
  width: 100%;
  min-width: 400px;
  display: block;
}
.mat-raised-button {
  width: 100%;
  height: 60px;
  margin-top: 15px;
}
.error-message {
  color: white;
  background-color: red;
}
```
```