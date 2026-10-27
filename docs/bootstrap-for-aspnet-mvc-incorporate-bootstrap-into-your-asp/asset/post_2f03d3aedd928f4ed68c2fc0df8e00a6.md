
```
closeButton.Attributes.Add("data-dismiss", "alert");
closeButton.InnerHtml = "&times;";
return closeButton;
}
```

14. The AddCloseButton method also uses the TagBuilder object to build up the HTML needed in order to render a close button inside the Bootstrap Alert class.

## Using the fluent HTML helper in a view

In order to use the fluent HTML helper inside a view, we need to add one final file called AlertHelper.cs to the Alerts folder. This class will add an extension method to the HtmlHelper object in order to render a Bootstrap Alert class. Add a new method called Alert to the class. This method should return an Alert object, and it will accept two parameters. The first parameter must be preceded with the this keyword and is used to indicate which class the extension will extend. The second parameter will be the actual text that will be displayed inside the Bootstrap Alert class.

The code for the Alert class is as follows:

```
public static class AlertHelper
{
    public static Alert Alert(this HtmlHelper html,string message)
    {
        return new Alert(message);
    }
}
```

Open a view and add the following markup to render a dismissible Bootstrap alert that will display a danger-styled message:

```
@Html.Alert("Core meltdown imminent").Danger().Dismissible()
```

The alert will look similar to the following screenshot in the browser:

![](img/371b9a67300a03b7b335eb8a6fe55d94_115_0.png)

## Creating self-closing helpers

Self-closing helpers are helpers that can contain HTML and Razor markup.
The built-in @Html.BeginForm() helper is an example of this type of helper.

In order to create this type of HTML helper, we'll need to create a helper class that implements the IDisposable interface. Using the IDisposable interface, we can write the element's closing tags when the object is disposed.

The Bootstrap Panel component is a perfect candidate for such a helper. To create the helper, we'll have to complete the following steps:

- 1. Create a new subfolder called Panels inside the Helpers folder in your project.
- 2. Open the Enums.cs file, and add a new item called PanelStyle:
- 3. Add a new class file called Panel.cs to the Panels folder.
- 4. Add a new private read-only field to the class called _writer.
- 5. Create a constructor for the Panel class that accepts three parameters. The first is a reference to the HtmlHelper object, the second specifies the title of the panel, and the third indicates the style of the panel.
- 6. Add the following code to the Panel class's constructor method:

```
public enum PanelStyle
{
    Default,
    Primary,
    Success,
    Info,
    Warning,
    Danger
}
```

```
public Panel(HtmlHelper helper, string title, Enums.PanelStyle style = Enums.PanelStyle.Default)
{
    _writer = helper.ViewContext.Writer;

    var panelDiv = new TagBuilder("div");
    panelDiv.AddCssClass("panel-" + style.ToString().ToLower());
    panelDiv.AddCssClass("panel");

    var panelHeadingDiv = new TagBuilder("div");
    panelHeadingDiv.AddCssClass("panel-heading");
```

```
var heading3Div = new TagBuilder("h3");
heading3Div.AddCssClass("panel-title");
heading3Div.SetInnerText(title);

var panelBodyDiv = new TagBuilder("div");
panelBodyDiv.AddCssClass("panel-body");

panelHeadingDiv.InnerHtml = heading3Div.ToString();

string html = string.Format("{0}{1}{2}", panelDiv.ToString(TagRenderMode.StartTag), panelHeadingDiv, panelBodyDiv.ToString(TagRenderMode.StartTag));
_writer.Write(html);
}
```

In this code, we've set the _writer field to the Writer property of the HtmlHelper objects' ViewContext property. By utilizing this property, we'll be able to write HTML to the current view.

We then built up the panel element's HTML markup using the TagBuilder object and finally, sent the result to the _writer object to output. Note that we used the overloaded ToString method of TagBuilder in order to specify that it should only generate the starting tags of the <div> element.

The two <div> elements with class names panel and panel-body will be closed in the Panel class's Dispose method, which is implemented in the following manner:

```
public void Dispose()
{
    _writer.Write("</div></div>");
}
```

## Using the self-closing helper in a view

To use the helper we created earlier in our view, we'll use the C# using keyword. The using keyword restricts the scope of an object and thus, works well with the IDisposable interface. In essence, as soon as the helper has finished rendering its HTML output, the Dispose method will automatically be invoked, thus closing the last two <div> element tags.

To use the helper in a view, use the following markup:

```
@using (Html.Panel("Title", Enums.PanelStyle.Success))
{
    <p>Hello World</p>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
}
```

The helper will generate the required HTML in order to show the following panel in the browser:

![](img/371b9a67300a03b7b335eb8a6fe55d94_118_0.png)

## Summary

In this chapter, we explored how you can decrease the amount of markup in your views, using HTML helpers. We also looked at how you can write helpers that will enable developers who are not familiar with the Bootstrap framework to use helpers to add styled components to their views.

In the next chapter, we'll dive into generating scaffolded views that are correctly styled using the Bootstrap styles and layouts.

## Creating T4 Templates to Scaffold Bootstrap Views

Visual Studio 2013 already includes code generators for ASP.NET MVC and Web API projects, and using these built-in scaffolding templates can drastically reduce the amount of time you spend on creating standard views and controller actions. When adding a standard controller to an ASP.NET MVC project in Visual Studio 2013, you're already using scaffolding, and this is indicated by the fact that the dialog window used to add a new controller's caption is **Add Scaffold**:

![](img/371b9a67300a03b7b335eb8a6fe55d94_120_0.png)

## T4 templates

T4 stands for Text Template Transformation Toolkit, which is Microsoft's template-based text-generation framework. T4 is a combination of control logic and text blocks, which can generate any type of text file, including C# or VB source files.

The T4 code generator is built into Visual Studio and is used to generate the built-in ASP.NET MVC templates, including the standard MVC and Web API controller templates.

> Oleg Sych wrote a number of very informative blog posts on how to use T4 templates and how to get optimal results by using it in your own development project. To read more, you can visit his blog at `http://bit.ly/OlegSychT4`.

Unfortunately, Visual Studio does not provide syntax highlighting or IntelliSense support when editing T4 templates, but there are free tools available that can add these features to Visual Studio for you.

## T4 tools

Since Visual Studio does not provide T4 IntelliSense by default, we'll need to install a third-party extension to enable this functionality for us. Fortunately, there are two free alternatives available. These are as follows:

- - The Devart T4 editor for Visual Studio (`http://bit.ly/DevArtT4`)
- - The tangible T4 editor (`http://bit.ly/TangibleT4`)

Both these extensions provide similar functionalities, but for the T4 examples in this book, we'll use the tangible T4 editor.

Another free extension that is vital in making the creation of T4 templates much easier is the **SideWaffle** extension for Visual Studio. SideWaffle will add a number of extremely-useful item templates to Visual Studio, which we'll use to build our own ASP.NET MVC scaffolded views. You can download SideWaffle from `http://www.sidewaffle.com/`.

> SideWaffle not only provides item templates for T4, but also offers a range of project and item templates as well as a host of very useful snippets. It is an actively-maintained open source project on GitHub and an indispensable tool for any .NET developer.

## The T4 syntax

T4 templates have a very specific syntax, which can appear somewhat daunting, but once you get used to it, it is relatively simple to understand and use. T4 uses three distinct tags to indicate code blocks.
