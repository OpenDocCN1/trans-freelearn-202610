# # Displaying data from a local or remote DataSource component in a Grid view

A Grid component can be created using the data present in the page or by referring to model data, that is, data in the JSON format. The Kendo UI library provides a DataSource component that can be used to store a model data. The DataSource component can be bound to a local data source or to a remote service that returns data in either the XML or JSON format.

# ## How to do it...

As mentioned in the previous recipe, we can either use a table element or a div element to construct a Grid:

```
```
<div id="grid">
</div>
```
```

Now, when we initialize the Grid using the kendoGrid function, we need to specify the configuration and data for the table:

```
```
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
```
```