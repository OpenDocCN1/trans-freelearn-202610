# # There's more...

The columns in the Grid can be resized and reordered by specifying the configuration details for these while initializing the Grid:

```
resizable: true,

reorderable: true
```

By mentioning resizable as true, the columns' width can be changed, as shown in the following screenshot:

| Movie | Year | Rating |
|---|---|---|
| The Shawshank Redemption | 1994 | 9.2 |
| The Godfather | 1972 | 9.2 |
| The Godfather - Part 2 | 1974 | 9 |
| Pulp Fiction | 1994 | 8.9 |

The columns in the Grid can be made reorderable by mentioning this attribute when initializing the Grid.

| Movie | Year | + Rating |
|---|---|---|
| The Shawshank Redemption | 1994 | 9.2 |
| The Godfather | 1972 | 9.2 |
| The Godfather - Part 2 | 1974 | 9 |
| Pulp Fiction | 1994 | 8.9 |

Here, the user is trying to reorder the last column by moving it to the second column.