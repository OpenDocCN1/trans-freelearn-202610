In the following example, we'll create a textbox for the models' `PostalCode` property, set its value element to 1.23, and its size to Large. We'll then specify the placeholder text and the format, and finally, we add a string to the front of the textbox:

```
```csharp
@Html.Bootstrap().TextBoxFor(m => m.MinimumTransactionValue).
    Value("1.23").Size(InputSize.Large).Placeholder("Enter Postal Code").
    Format("{0:0.00}").Prepend("US")
```
```

The resulting textbox should look similar to the following screenshot in your browser:

![](bbox=[0.362, 0.318, 0.638, 0.384])

To create a text area input using the `TwitterBootstrapMVC` library, you can use the `TextAreaFor` helper. This helper supports two additional methods with which you can specify the number of columns and rows for the text area. In the following example, we'll create a text area for the models' `Address` property and set its columns value to 10 and its rows value to 5:

```
```csharp
@Html.Bootstrap().TextAreaFor(m => m.Address).Columns(10).Rows(5)
```
```

The text areas should be rendered similar to the following screenshot in your browser:

![](bbox=[0.375, 0.566, 0.625, 0.698])