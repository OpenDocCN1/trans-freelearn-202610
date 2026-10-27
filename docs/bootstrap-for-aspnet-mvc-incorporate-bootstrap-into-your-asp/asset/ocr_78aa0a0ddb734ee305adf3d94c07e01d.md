```
<h1>Customers <small>Our Customers</small></h1>
</div>

<ol class="breadcrumb">
    <li>@Html.ActionLink("Home", "Index", "Home")</li>
    <li class="active">Customers</li>
</ol>
<table class="table table table-striped table-hover">
    <thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.
                CustomerCode)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.
                CompanyName)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.
                ContactName)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.City)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Country)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Phone)
            </th>
        </tr>
    </thead>
    <tbody>
        @foreach (var item in Model)
        {
            <tr>
                <td>
                    @Html.ActionLink(item.CustomerCode,
                    "Edit", new { id = item.CustomerId })
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.
                    CompanyName)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.
                    ContactName)
                </td>
                <td>.
```