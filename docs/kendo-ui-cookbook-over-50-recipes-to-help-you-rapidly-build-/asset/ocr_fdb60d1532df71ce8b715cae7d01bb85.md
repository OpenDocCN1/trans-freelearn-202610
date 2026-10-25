# # Updating the pointer value using a Slider widget

The `pointer` value is set when the Radial Gauge widget is initialized. It is possible to change the `pointer` value of the widget at runtime using a Slider widget. The changes in the Slider widget can be observed, and the `pointer` value of the Radial Gauge can be updated accordingly.

Let's use the same Radial Gauge widget mentioned in the previous recipe. A Slider widget is created using an input element:

```
```
<input id="slider" value="0" />
```
```

The next step is to initialize the previously mentioned input element to a Slider widget:

```
```
$(\'#slider\').kendoSlider({
    min: 0,
    max: 200,
    showButtons: false,
    smallStep: 10,
    tickPlacement: \'none\',
    change: updateRadialGuage
});
```
```

The `min` and `max` values specify the range of values that can be set for the slider. The `smallStep` attribute specifies the minimum increment value of the slider. The `change` attribute specifies the function that should be invoked when the `slider` value changes.

The `updateRadialGuage` function should then update the value of the pointer in the Radial Gauge widget:

```
```
function updateRadialGuage() {
    $(\'#chart\').data(\'kendoRadialGauge\')
        .value($(\'#slider\').val());
}
```
```

The function gets the instance of the widget and then sets its value to the value obtained from the Slider widget.