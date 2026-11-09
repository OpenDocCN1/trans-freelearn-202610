A collection of data can be called semi-structured when its data values are labeled, but their order does not matter, and in fact, some of the data values may even be missing. A JSON file is just a collection of attribute-value pairs, such as this:

```
{
  "firstName": "Sally",
  "birthDate": "1971-09-16",
  "faveColor": "light\"Carolina\" blue",
  "salary":129000
}
```

The attributes are on the left-hand side of the colon, and the values are on the right-hand side. Each attribute is separated by a comma. The entire entity is enclosed in curly braces.

A few things about JSON are similar to delimited files, and a few things are different. First, string values must be double-quoted, and therefore, any double quotes internal to a string must be escaped with a backslash. (The escape character \, when used as a regular character, must also be escaped!)

In JSON, commas cannot be included in numeric values unless the values are treated as strings and properly quoted. (Be careful when stringifying numbers, though – are you sure you want to do that? Then, refer to the Data types section in this chapter, which can help you with these issues.)

```
{
  "firstName": "Sally",
  "birthDate": "1971-09-16",
  "faveColor": "light\"Carolina\" blue",
  "salary": "129,000"
}
```

Multivalue attributes are acceptable, as are hierarchical values (neither of these are available very easily with delimited files). Here is an example of a JSON file with a multivalue attribute for pet and a hierarchy of data for jobTitle. Note that we've moved the salary data inside this new job hierarchy:

```
{
  "firstName": "Sally",
  "birthDate": "1971-09-16",
  "faveColor": "light\"Carolina\" blue",
  "pet":
  [
    {
      "type": "dog",
      "name": "Fido"

```