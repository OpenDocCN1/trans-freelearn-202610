A little experimentation reveals that the default Minimum and Maximum values are 0 and 1 for the Slider, but 0 and 100 for the Stepper, and of course you can change them. The Stepper has a default Increment value of 1.

If you don’t like the many decimal places displayed for the Slider on the iPhone and Windows Phone, you can easily fix that with a formatting string appropriate for double:

```
sliderValueLabel.Text = args.NewValue.ToString("F2");
```

Suppose you want to initialize the Value property of the Slider after instantiating it:

```
Slider slider = new Slider
{
    VerticalOptions = LayoutOptions.EndAndExpand
};
slider.ValueChanged += OnSliderValueChanged;
slider.Value = 0.5;
```

This code will cause a NullReferenceException to be raised. The problem is that setting the Value property triggers a call to the OnSliderValueChanged handler, which references a Label that hasn’t been created yet. You can avoid this problem by setting the Value property before attaching the event handler, or after instantiating the Label. Or, the event handler can check if the Label object is not null before attempting to set its Text property.

Instead of setting the Value property of Slider to 0.5, try this:

```
slider.Value = 0;
```

This statement will not result in a NullReferenceException because it will not trigger the ValueChanged event to be fired. The event is called ValueChanged and not ValueSet for a reason: The event is not fired unless the Value property has actually changed.

If you want the Label views to display the initial values at program startup, your code can set the Text properties to the appropriate values when the Label views are created. Or, you can “fake” an event by calling the event handlers directly, perhaps at the end of the constructor after everything has been created.

Or, you can use data bindings.

# Data binding

The SliderStepperSwitch program sets event handlers so that the Text property of a Label tracks the value of the interactive view. Tasks such as this can be automated with the same data-binding techniques you saw in the NoteTaker programs in Chapter 3 and 4.

As you learned at that time, data bindings link the value of a property of one object with the value of a property of another object to keep the values synchronized. These two objects are distinguished as