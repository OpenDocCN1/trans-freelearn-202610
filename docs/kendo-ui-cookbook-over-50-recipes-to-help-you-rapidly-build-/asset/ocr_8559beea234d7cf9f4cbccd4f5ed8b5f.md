```
field : 'year',
 title : 'Year'
},{{
 field : 'rating',
 title : 'Rating'
}},{
 command: ['edit', 'destroy']
}}
]
```

Here, the `columns` configuration lists the various fields and mentions the `edit` and `destroy` commands. This will add the **Edit** and **Delete** buttons for each row in the Grid. These buttons should be bound to a remote service so that when the user updates or deletes a record, the service will update the same in the database. In earlier recipes, we looked at using the `DataSource` object with a `transport` attribute. Here, you can use the same attribute to specify the endpoint for each of the actions, as shown in the following code snippet:

```
dataSource: {
 transport: {
 read: '/test',
 create: {
 url: '/test',
 type: 'POST'
 },
 update: {
 url: '/test',
 type: 'PUT'
 },
 destroy: {
 url: '/test',
 type: 'DELETE'
 }
 },
 schema: {
 model: {
 id: "movieName",
 fields: {
 movieName: { type: "string" },
 year: { type: "number" },
 rating: { type: "number" }
 }
 }
 }
 }
}
```