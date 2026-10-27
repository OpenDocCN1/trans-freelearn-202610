```
<th>
    Units In Stock
</th>
<th>
    Discontinued
</th>
<th></th>
</tr>
</thead>
<tbody>
@for each (var item in Model)
{
    <tr class="@item.Status">
        <td>
            @Html.DisplayFor(modelItem => item.ProductName)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.UnitPrice)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.UnitsInStock)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Discontinued)
        </td>
        <td>
            @Html.ActionLink("Edit", "Edit", new { /* id=item.PrimaryKey */ }) |
            @Html.ActionLink("Details", "Details", new { /* id=item.PrimaryKey */ }) |
            @Html.ActionLink("Delete", "Delete", new { /* id=item.PrimaryKey */ })
        </td>
    </tr>
}
</tbody>
</table>
```