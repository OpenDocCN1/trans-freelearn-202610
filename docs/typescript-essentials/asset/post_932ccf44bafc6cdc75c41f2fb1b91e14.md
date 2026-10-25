
whether or not the code should be minified.

## Note

For more information about the r.js optimizer, please visit http://requirejs.org/docs/optimization.html.

For the sake of readability, we will leave it at none for now. When we actually want to deploy our code, we can simply comment this option out and all of our code will be placed on a single line in a single file. The final thing we must do to generate our optimized JavaScript output file is create a simple PowerShell script that will run the optimizer and provide our options:

```
cls
write-host '-Building Drawing Application'
node Scripts/r.js -o buildSingleFile.js
```

In this script, we clear the current screen, log some output, and finally use Node.js to run the RequireJS optimizer. If everything goes as expected, the optimizer will provide tracing output, letting us know that all of the modules required for our application to work have been included in the optimization. The following screenshot shows the output of running our script and as you can see all of the JavaScript dependencies have been included in the build:

![](img/83a7f7e1723158e56008cf70304a8da4_223_0.png)

The final piece of this process is modifying our HTML page to point at the resulting output file. Before we can do this, we must understand the output from the PowerShell script we just ran. As we discussed earlier, there are three parameters for AMD modules;

## Styling the application

So far, we have only covered the way we develop the source code for our applications. However, styling the application is just as important as the functionality. The code that we write can be completely free of bugs, but if the appearance and feel of the application is poor, users will not adopt it. Traditionally, when developing web applications using HTML pages, we would use Cascading Style Sheets (CSS) to style our objects. However, similar to the way TypeScript improves JavaScript development, a language called LESS has been developed that will compile into plain CSS. LESS is available for standalone download from http://lesscss.org/, or in the case of Visual Studio, it can be installed with Mads Kristensen’s Web Essentials plugin. This plugin is available through the Extensions and Updates window just like the NuGet Package Manager was.

![](img/83a7f7e1723158e56008cf70304a8da4_227_0.png)

Once this has been installed, we can add a LESS style sheet to our application. LESS has a lot of incredibly useful features that will help us create a friendly user interface in a more robust manner. We have the ability to declare variables and nest styles within each other. Let’s use LESS to improve the feel of our application. Currently, a user has no visual cues informing them what tool they are currently working with. We could use some code to change the cursor type when a button is clicked; however, this can be overridden by other elements on the page, including the CanvasEngine class that our application relies on. Instead, we will use Knockout bindings combined with generated CSS styles. First, we will need to create a new property on the IToolSelectionControl interface and the ToolSelectionControl class to store whether or not the current control is selected or not:

```
import ControlsTypes = require('Scripts/Controls/ControlsTypes');
import UserControlBase = require('Scripts/Controls/UserControlBase');
import DrawingTypes = require('Scripts/Drawing/DrawingTypes');
class ToolSelectionControl extends UserControlBase
    implements ControlsTypes.IToolSelectionControl {
    public buttonText: KnockoutObservable<string> = ko.observable("");
    public isSelected: KnockoutComputed<boolean> = null;
    constructor(id: string, public toolType: DrawingTypes.DrawingToolType,
        public observable: KnockoutObservable<DrawingTypes.DrawingToolType>) {
        super(id);
        this.templateName = "ButtonTemplate";
        this.buttonText(DrawingTypes.DrawingToolType[this.toolType]);
        this.isSelected = ko.computed(() => {
            return this.observable() == this.toolType;
        });
    }
    public click(viewModel: any, event: JQueryEventObject) {
        this.observable(this.toolType);
    }
```

```
}
}
export = ToolSelectionControl;
```

As you can see, we created a new KnockoutComputed object that will return a Boolean and that our HTML will be bound to. Instead of storing an instance value like observables do, computed objects run a function that is responsible for either setting or returning the value. In this case, we check whether the value of the observable the control is supposed to update is the same as the tool that this control is associated with. If they are the same, then a true value will be returned; otherwise, the result will be false. By including another observable object within the body of this function, Knockout will create a subscription to this object, and any time its value is changed, this function will be re-evaluated. This keeps us from having to write any additional logic to update an observable whenever one of these controls is clicked. Binding to computed objects is the same as binding to observables as they share a common super class.

```
<script type="text/html" id="ButtonTemplate">
    <button class="tool-select-button" data-bind="text: buttonText, click: click,
        css: { 'selected': isSelected }" />
</script>
```

In the preceding screenshot, you can see we have added a class to our button that we will be able to provide styles for and then the additional Knockout binding. This binding will add the 'selected' class to the button element if the computed object returns a true value. Now we need to define these styles and include them in our application. One of the most valuable features of LESS is the ability to define variables that can be reused through all of your LESS files. In the following example, we define several colors that we want to use throughout our application:

```
@selectionColor: #1111DD;
@accentColor: white;
@borderColor: black;
```

The selectionColor variable will be used to denote the color of the tool that is currently selected, while the accentColor and borderColor variables will be used for a variety of purposes. Rather than explicitly defining these colors for each style and having to change every place they are used, we will simply be able to change the value of these variables and the entire look of our application can change. To use these variables, we simply import the .less file that contains them and they will be available:

```
@import 'ApplicationColors.less';
.tool-select-button {
    background-color: @accentColor;
    border-color: @borderColor;
    &.selected {
        background-color: @selectionColor;
        color: @accentColor;
    }
}
input[type=color] {
```

```
border-color: @borderColor;
}
canvas {
    border: 4px solid @borderColor;
}
```

We use our variables to define the values that our styles will have. The buttons that now have the .tool-select-button class will be given new background and border colors. There is also a nested style shown, which when compiled into CSS will create another style that represents the union between these two styles. The final style puts a border around the color picker. When the LESS code is compiled, the resulting CSS will look similar to the following code:

```
.tool-select-button {
    background-color: #ffffff;
    border-color: #000000;
}
.tool-select-button.selected {
    background-color: #1111dd;
    color: #ffffff;
}
input[type=color] {
    border-color: #000000;
}
canvas {
    border: 4px solid #000000;
}
```

Notice that all of the variables have been removed and replaced with the values they represent. LESS variables have no representation in CSS, so they behave similar to interfaces in TypeScript in that they have no compiled output. If a LESS file is imported that does contain styles, all of those styles will be included in the resulting CSS file as a merged set of styles. Now we can simply modify our HTML to link our style sheet and our application will have a better user experience.
