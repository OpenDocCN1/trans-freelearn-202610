# Styling the application

So far, we have only covered the way we develop the source code for our applications. However, styling the application is just as important as the functionality. The code that we write can be completely free of bugs, but if the appearance and feel of the application is poor, users will not adopt it. Traditionally, when developing web applications using HTML pages, we would use Cascading Style Sheets (CSS) to style our objects. However, similar to the way TypeScript improves JavaScript development, a language called LESS has been developed that will compile into plain CSS. LESS is available for standalone download from http://lesscss.org/, or in the case of Visual Studio, it can be installed with Mads Kristensen’s Web Essentials plugin. This plugin is available through the Extensions and Updates window just like the NuGet Package Manager was.

![](bbox=[0.058, 0.298, 0.612, 0.366])

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