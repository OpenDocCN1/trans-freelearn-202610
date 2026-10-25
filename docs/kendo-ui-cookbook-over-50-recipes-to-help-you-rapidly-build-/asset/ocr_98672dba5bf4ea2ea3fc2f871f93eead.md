Here, the Grid is sorted by the second column, Year, first and then by the third column, Rating. A downward arrow indicates that the column is sorted in the descending order. Here, the Grid is sorted by listing the recent movies that have high ratings.

# # Using filters to display data that matches certain criteria in the Grid

Similar to sorting columns in a Grid, you can update the Grid content by applying a filter. By applying a filter on the column, users will be able to search relevant data in the Grid that matches some criteria.

# ## How to do it...

To make the columns in the Grid filterable, set the configuration option, `filterable`, to `true`, as shown in the following line of code. Similar to the `sortable` option, all columns in the Grid will be available for filtering by default. If you want some of the columns to not be available for filtering, then set `filterable` as `false` under columns.

```
```
filterable: true
```
```

# ## How it works...

When you mark the columns for filtering, you will see a **filter** icon for each of the columns in the Grid.

| Movie                 |   Year |   Rating |
|-----------------------|-------:|---------:|
| The Shawshank Redemption |   1994 |      9.2 |
| The Godfather         |   1972 |      9.2 |
| The Godfather - Part 2 |   1974 |      9   |
| Pulp Fiction          |   1994 |      8.9 |
| The Good, the Bad and the Ugly |   1966 |      8.9 |