```
"name": "Smith Cooper",
"manages": [
{
"designation": "Director",
"name": "Andrew Walker"
},
{
"designation": "Director",
"name": "Robert Thompson"
}
],
},
{
"designation": "Vice President",
"name": "Harris Jones",
"manages": [
{
"designation": "Director",
"name": "Edward Collins"
},
{
"designation": "Director",
"name": "Thomas Wang"
}
]
}
];
```

In the preceding code snippet, we are creating a hierarchical structure where each object contains three fields: designation, name, and manages. The manages field represents the child nodes for a particular node. Each node will show you the name and designation fields, and then connectors would be added to represent the ones mentioned in the manages collection.

The next step is to specify the DataSource object details when creating a diagram. In the DataSource option, you should also specify the schema details to let the framework know which attribute is to be considered for adding child nodes, as shown in the following code snippet:

```
$("#chart").kendoDiagram({
dataSource: {
data: data,
schema: {

```