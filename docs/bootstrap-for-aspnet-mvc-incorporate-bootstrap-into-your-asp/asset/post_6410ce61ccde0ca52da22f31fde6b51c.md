
## Using Bootstrap Components

> SignalR is a library for ASP.NET; this library makes it very easy to add real-time functionality and feedback to your applications. You can read more about it at www.signalr.net.

2. Next, add a new folder called `Hubs` to your project, and add a new class called `ProgressbarHub.cs` to the folder. Change its code to the following code:

```csharp
using System.Threading;
using Microsoft.AspNet.SignalR;

namespace Northwind.Web.Hubs
{
    public class ProgressbarHub : Hub
    {
        public void SendProgress()
        {
            for (int i = 0; i <= 100; i++)
            {
                Thread.Sleep(50);
                Clients.Caller.sendMessage(i + "%");
            }
        }
    }
}
```

3. In the preceding code, our class inherits from the SignalR Hub class and contains a single method called `SendProgress`, which will increment a variable and send the value to any SignalR clients using the `sendMessage` method.

4. Next, we add a reference to the SignalR virtual path. Open the `_Layout.cshtml` file, and add the following line to the bottom of the file:

```html
<script src="~/signalr/hubs"></script>
```

5. We'll also add a reference to the SignalR JavaScript library in our existing bundles. Open the `BundleConfig.cs` file located in the `App_Start` folder, and change the jquery bundle to include the SignalR library as follows:

```csharp
bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
    "~/Scripts/jquery-{version}.js",
    "~/Scripts/jquery.signalR-2.0.3.js"));
```

6. Finally, on the page where we have our progress bars, add the following script inside the Scripts section:

```csharp
@section scripts
{
    <script type="text/javascript" language="javascript">
        $("#start").click(function () {
            $(".progress-bar").width('0%');
            var progressNotifier = $.connection.progressbarHub;
            progressNotifier.client.sendMessage = function (message) {
                updateProgress(message);
            };

            $.connection.hub.start().done(function () {
                progressNotifier.server.sendProgress();
            });
        });

        function updateProgress(message) {
            $(".progress-bar").width(message);
            $("#progressbarTitle").html(message + ' Complete');
        }
    </script>
}
```

The preceding code will be executed when the user clicks on a button with an ID of start. It will first set all the progress bars on the page's width to 0 and initialize a connection to the server. We then set the function name that will be called from the server side. In this scenario, the JavaScript function's name is updateProgress.

Finally, we establish a connection to the server and start the server-side operation. As you can see, we invoked the sendProgress function—this is the name of the method declared inside the ProgressbarHub class.

As a result of the code, you will see that all the progress bars on the page will increase their values from 0 to 100.

## Summary

In this chapter, we explored the many different Bootstrap components as well as learned how to use them in your ASP.NET MVC project. We also looked at NuGet packages to aid you in creating paged lists and to dynamically update progress bars.

In the next chapter, we'll delve deeper into the Bootstrap components by investigating how to add more interactivity to your site using the Bootstrap JavaScript plugins.

## Using Bootstrap JavaScript Plugins

In order to use Bootstrap plugins, we'll need to include the `bootstrap.js` or `bootstrap.min.js` file in our project. This file contains all the Bootstrap plugins, but if you do not intend to use every plugin in your project, it is a good idea to customize which components are to be included in the `bootstrap.js` file. This will make the size of the Bootstrap library smaller and allow the website to load faster. To access these plugins, navigate to http://getbootstrap.com/customize/ and select the plugins that you'll be using in your project.

### Data attributes versus the programmatic API

Bootstrap offers the ability to use its plugins entirely through the HTML markup. This means that in order to use most of the plugins, you do not need to write a single line of JavaScript. Using data attributes is the recommended approach and should be your first option when using Bootstrap plugins.

For example, to allow an `alert` element to be dismissible, you'll add the `data-dismiss="alert"` attribute to either a `button` or `anchor` element, as illustrated in the following code:

```html
<div class="alert alert-danger">
  <button data-dismiss="alert" class="close" type="button">×</button>
  <strong>Warning</strong> Shuttle launch in t-minus 10 seconds.
</div>
```

You also have the option to perform the same action using the programmatic API via JavaScript. The following code uses jQuery to close a specific `alert` element when the user clicks on a button:

```html
<button class="close" type="button" onclick="$('#myalert').alert('close')">×</button>
```

### Cascading dropdowns

You can add drop-down menus to almost any Bootstrap component using the dropdown plugin. A cascading dropdown is a dropdown that updates its data based on a value selected in another dropdown. To add cascading dropdowns, perform the following steps:

1. We'll add drop-down menus to two buttons which will list the region and territory data that was retrieved from a database. When the user selects a region, the Territory drop-down list will update to show only those territories associated with the selected region. The following screenshot illustrates the end result:

![](img/371b9a67300a03b7b335eb8a6fe55d94_89_0.png)

2. The two drop-down menus will be used in our Edit Customer view, and in order for the list of territories and regions to be accessible in our view, we'll first add them to the ViewBag object using the following code:

```csharp
public ActionResult Edit(int id)
{
    var model = _context.Customers.First(c => c.CustomerId == id);
    ViewBag.RegionId = new SelectList(_context.Regions,
    "RegionId", "RegionDescription", model.RegionId);
    ViewBag.TerritoryId = new SelectList(_context.Territories.Where(t => t.RegionId == model.RegionId), "TerritoryId",
    "TerritoryDescription", model.TerritoryId);
    return View(model);
}
```

In this code, we first loaded the requested customer from the database and then added a list of regions and territories from the database to the ViewBag object. You will notice that we only retrieved the territories in the selected customer's region.

3. Next, we add the Razor markup to our view to create two buttons with drop-down menus. In the following code, we'll create two new Bootstrap form groups that contain a label for the region and territory properties as well as hidden elements that will carry the selected region and territory ID. We iterate over the regions and territories that were passed to the view via the ViewBag object and build up the menu items.

```html
<div class="form-group">
    @Html.LabelFor(model => model.RegionId,
        new { @class = "control-label col-md-2" })
    @Html.HiddenFor(model => model.RegionId)
    <div class="col-md-10">
        <div id="regiondropdown" class="dropdown">
            <button id="regionbutton" class="btn btn-sm btn-info"
                    data-toggle="dropdown">@Model.Region.RegionDescription</button>
            <ul class="dropdown-menu">
                @foreach (var item in ViewBag.RegionId)
                {
                    <li><a href="#" tabindex="-1" data-value="@item.Value">@item.Text</a></li>
                }
            </ul>
        </div>
    </div>
</div>
<div class="form-group">
    @Html.LabelFor(model => model.TerritoryId, new { @class =
        "control-label col-md-2" })
    @Html.HiddenFor(model => model.TerritoryId)
    <div class="col-md-10">
        <div id="territorydropdown" class="dropdown">
            <button id="territorybutton" class="btn btn-sm btn-info" data-toggle="dropdown">@Model.Territory.TerritoryDescription</button>
            <ul id="territories" class="dropdown-menu">
                @foreach (var item in ViewBag.TerritoryId)
                {
                    <li><a href="#" tabindex="-1" data-value="@item.Value">@item.Text</a></li>
                }
            </ul>
        </div>
    </div>
</div>
```
