To get around this, define a schema in the DataSource object, as shown in the following code snippet:

```
dataSource: {
    transport: {
        read: 'http://localhost/kendo/code/chapter2/remote.json'
    },

    schema: {
        model: {
            fields: {
                movieName: { type: "string" },
                year: { type: "number" },
                rating: { type: "number" }
            }
        }
    }
}
```

This will instruct the framework to use the correct data type when applying the filter on columns.

| Movie | Year | Rating |
|---|---|---|
| The Shawshank Redemption | 1994 | Show items with value that: |
| The Godfather | 1972 | Is equal to ▼ |
| The Godfather - Part 2 | 1974 | 1,994.00 ▲ ▼ |
| Pulp Fiction | 1994 | And ▼ |
| The Good, the Bad and the Ugly | 1966 | Is equal to ▼ ▲ ▼ |
| | | Filter Clear |

Here, the framework sees that the Year field is of number type and shows the filter criteria with different conditions in the dropdown. The list contains the Is equal to, Is not equal to, Is greater than, Is greater than or equal to, Is less than, and Is less than or equal to conditions.