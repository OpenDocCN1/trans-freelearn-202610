# Using Bootstrap JavaScript Plugins

In this code, we first loaded the requested customer from the database and then added a list of regions and territories from the database to the ViewBag object. You will notice that we only retrieved the territories in the selected customer's region.

+   3. Next, we add the Razor markup to our view to create two buttons with drop-down menus. In the following code, we'll create two new Bootstrap form groups that contain a label for the region and territory properties as well as hidden elements that will carry the selected region and territory ID. We iterate over the regions and territories that were passed to the view via the ViewBag object and build up the menu items.

```
```html
<div class="form-group">
    @Html.LabelFor(model => model.RegionId,
        new { @class = "control-label col-md-2" })
    @Html.HiddenFor(model => model.RegionId)
    <div class="col-md-10">
        <div id="regiondropdown" class="dropdown">
            <button id="regionbutton" class="btn btn-sm btn-info"
                    data-toggle="dropdown">@Model.Region.
                RegionDescription</button>
            <ul class="dropdown-menu">
                @foreach (var item in ViewBag.RegionId)
                {
                    <li><a href="#" tabindex="-1" data-value="@
                        item.Value">@item.Text</a></li>
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
            <button id="territorybutton" class="btn btn-
                sm btn-info" data-toggle="dropdown">@Model.Territory.
                TerritoryDescription</button>
            <ul id="territories" class="dropdown-menu">
                @foreach (var item in ViewBag.TerritoryId)
                {
                    <li><a href="#" tabindex="-1" data-value="@
                        item.Value">@item.Text</a></li>
                }
            </ul>
        </div>
    </div>
</div>
```
```