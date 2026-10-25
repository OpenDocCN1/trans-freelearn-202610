# # How it works...

As mentioned previously, we chose a div element to create a Grid. This element would serve as a container for the Grid. While initializing the Grid, the framework would look at the provided configuration and generate the markup, which will then be inserted inside the div element. It creates two div elements—one for the header and the other for the content. The header is shown as follows:

```
`<div class="k-grid-header" style="padding-right: 16px;">
<div class="k-grid-header-wrap">
<table role="grid">
<colgroup>
<col>
<col>
<col>
</colgroup>
<thead>
<tr>
<th role="columnheader"
data-field="movieName"
data-title="Movie"
class="k-header">
Movie
</th>
<th role="columnheader"
data-field="year"
data-title="Year"
class="k-header">
Year
</th>
<th role="columnheader"
data-field="rating"
data-title="Rating"
class="k-header">
Rating
</th>
</tr>
</thead>
</table>
</div>
</div>`
```