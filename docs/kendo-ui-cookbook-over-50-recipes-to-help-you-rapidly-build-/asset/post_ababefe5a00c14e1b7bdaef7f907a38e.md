

## How it works...

When you render the page, you will see the previously mentioned table data being displayed inside a Grid, as shown in the following screenshot:

| Employee ID | Full Name | Year of Joining | Designation | Extension No. |
|-------------|-----------|-----------------|-------------|---------------|
| 10001       | Sagar H Ganatra | 2008 | Software Architect | 49523 |
| 7008        | Adam Wayne | 2000 | Engineering Manager | 34890 |
| 10298       | Nick Taylor | 2012 | Software Engineer | 56823 |
| 10677       | James Thompson | 2010 | Senior Software Engineer | 48999 |
| 6745        | Andrew Jones | 2003 | Senior Engineering Manager | 48999 |

As seen in the previous screenshot, the columns are automatically adjusted, and each column has the same width. The previously mentioned Grid occupies the entire viewport width, unless it is wrapped inside a container with a fixed width. The column size can be changed by using the colgroup tag:

```html
<table id="grid">
<colgroup>
<col style="width:130px" />
<col />
<col style="width:130px" />
<col />
<col style="width:130px" />
</colgroup>
<thead>
</table>
```

Here, the width for the Employee ID, Year of Joining, and Extension No columns is specified as 130px. Now, when Grid is initialized, these columns will have the specified width, and the two other columns, namely Full Name and Designation, will share the remaining space. The updated Grid is shown as follows:

| Employee ID | Full Name | Year of Joining | Designation | Extension No. |
|---|---|---|---|---|
| 10001 | Sagar H Ganatra | 2008 | Software Architect | 49523 |
| 7008 | Adam Wayne | 2000 | Engineering Manager | 34890 |
| 10298 | Nick Taylor | 2012 | Software Engineer | 56823 |
| 10677 | James Thompson | 2010 | Senior Software Engineer | 48999 |
| 6745 | Andrew Jones | 2003 | Senior Engineering Manager | 48999 |

As mentioned previously, the second and fourth columns in the Grid have the same width.

## There's more...

Note the look and feel applied to the Grid. Alternate rows in the Grid have different colors; even the Grid header has a color applied to it. This is because of the inclusion of the kendo.default.min.css style sheet. The library comes with several themes, which you can apply by removing the reference to the default style sheet and then adding the reference to one of the themes. For example, the library comes with the moonlight theme. By adding a reference to kendo.moonlight.min.css, the look and feel of the Grid will change.

| Employee ID | Full Name | Year of Joining | Designation | Extension No. |
|---|---|---|---|---|
| 10001 | Sagar H Ganatra | 2008 | Software Architect | 49523 |
| 7008 | Adam Wayne | 2000 | Engineering Manager | 34890 |
| 10298 | Nick Taylor | 2012 | Software Engineer | 56823 |
| 10677 | James Thompson | 2010 | Senior Software Engineer | 48999 |
| 6745 | Andrew Jones | 2003 | Senior Engineering Manager | 48999 |

There are several other themes that come with the library. It is also possible to create your own theme; please refer to the following link for the same:
http://demos.kendoui.com/themebuilder/web.html

## Displaying data from a local or remote DataSource component in a Grid view

A Grid component can be created using the data present in the page or by referring to model data, that is, data in the JSON format. The Kendo UI library provides a DataSource component that can be used to store a model data. The DataSource component can be bound to a local data source or to a remote service that returns data in either the XML or JSON format.

### How to do it...

As mentioned in the previous recipe, we can either use a table element or a div element to construct a Grid:

```html
<div id="grid">
</div>
```

Now, when we initialize the Grid using the kendoGrid function, we need to specify the configuration and data for the table:

```javascript
$("#grid").kendoGrid({
    columns: [
        {
            field : 'movieName',
            title : 'Movie'
        },
        {
            field : 'year',
            title : 'Year'
        },
        {
            field : 'rating',
            title : 'Rating'
        }
    ],
    dataSource: [
        {
            movieName : 'The Shawshank Redemption',
            year      : 1994,
            rating    : 9.2
        },
        {
            movieName : 'The Godfather',
            year    : 1972,
            rating  : 9.2
        },
        {
            movieName : 'The Godfather - Part 2',
            year    : 1974,
            rating  : 9.0
        },
        {
            movieName : 'Pulp Fiction',
            year    : 1994,
            rating  : 8.9
        },
        {
            movieName : 'The Good, the Bad and the Ugly',
            year    : 1966,
            rating  : 8.9
        }
    ]
});
```

In the preceding code snippet, the Grid is being initialized with two objects, namely `columns` and `dataSource`. The `columns` object is a collection of JavaScript objects that specify the column configuration. The configuration contains two properties, `field` and `title`. The `field` property refers to the field in `dataSource` that the column is bound to and `title` specifies the text to be displayed as a header in the Grid.

The `dataSource` object is also a collection of JavaScript objects that contains the data that will be displayed in the Grid. In this example, there are five objects and hence, the Grid will contain five rows.

> Please note, the `columns` object can be an array of strings as well:
> `columns: ['movieName', 'year', 'rating']`
> In this case, the entries in the columns array should match the field name in the `dataSource` object. The title for the columns will be the same as the field names.

## How it works...

As mentioned previously, we chose a div element to create a Grid. This element would serve as a container for the Grid. While initializing the Grid, the framework would look at the provided configuration and generate the markup, which will then be inserted inside the div element. It creates two div elements—one for the header and the other for the content. The header is shown as follows:

```html
<div class="k-grid-header" style="padding-right: 16px;">
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
</div>
```

Based on the configuration provided in the columns object, a table that contains the header columns is generated (the thead and th tags). To display the Grid data, the library inserts another div element:

```html
<div class="k-grid-content">
<table role="grid">
<colgroup>
<col>
<col>
<col>
</colgroup>
<tbody>
<tr data-uid="d71ac3fd-9769-4161-9277-432f151490c3"
role="row">
<td role="gridcell">
The Shawshank Redemption</td>
<td role="gridcell">1994</td>
<td role="gridcell">9.2</td>
</tr>
<tr class="k-alt"
data-uid="3240f52d-f4bb-4a2c-9985-b6ce44d75be6"
role="row">
<td role="gridcell">The Godfather</td>
<td role="gridcell">1972</td>
<td role="gridcell">9.2</td>
</tr>
...
</tbody>
</table>
</div>
```

The preceding code contains a table with rows that display data from the dataSource object. Now, when you view the Grid on the page, it will be the same as the Grid that was explained in the previous recipe. This is shown in the following screenshot:

| Movie | Year | Rating |
|---|---|---|
| The Shawshank Redemption | 1994 | 9.2 |
| The Godfather | 1972 | 9.2 |
| The Godfather - Part 2 | 1974 | 9 |
| Pulp Fiction | 1994 | 8.9 |
| The Good, the Bad and the Ugly | 1966 | 8.9 |

## There's more...

The `dataSource` object in this example contained the local data, that is, the data for the Grid was mentioned right within the JavaScript object. However, in most of the scenarios, the data will be fetched from a remote service. The `dataSource` object can be used to fetch data from a remote service by specifying the URL for it:

```javascript
$("#grid").kendoGrid({
    columns: [
    {
    field : 'movieName',
    title : 'Movie'
    },
    {
    field : 'year',
    title : 'Year'
    },
    {
    field : 'rating',
    title : 'Rating'
    }
    ],
    dataSource: {
    transport: {
    read: 'http://localhost/kendo/code/chapter2/remote.json'
    }
    }
    });
```

In the preceding code snippet, the `DataSource` object has changed. It now contains a reference to the URL of a remote service (under the `transport` option). When the Grid is initialized, a `GET` request to the specified URL is sent and the data received as a response is then used to populate the content of the Grid.

## Sorting data in a Grid using a selected column

The Kendo UI Grid comes with several features, such as sorting by selected columns, pagination, grouping, and scrolling. These configuration options can be specified at the Grid configuration level and at the column level as well.

