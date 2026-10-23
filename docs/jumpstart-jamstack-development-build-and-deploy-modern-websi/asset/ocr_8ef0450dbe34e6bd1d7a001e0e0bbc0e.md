Similarly, we can query an event using the venue name, as shown in the following code snippet. This is similar to the example shown in *Chapter 6, Sanity's GraphQL Playground*:

```
```
query MyQuery {
  sanityEvent(venue: {name: {eq: "Will's Pub"}}) {
    name
  }
}
```
```

Similarly, the results will be as shown with the name of the event, as requested in the name field from the preceding code snippet, as follows:

```
```
"data": {
    "sanityEvent": {
      "name": "The Applesead Cast"
    "venue": {
      "name": "Will's Pub"
    }
  }
}
```
```

These two examples show clearly how GraphQL is used within Gatsby, providing an easy way to query a Sanity source via GraphQL.

# # Summary

In this chapter, we learned about the Gatsby Sanity source plugin and how it uses GraphQL to interact with and import content from Sanity. We learned how to use GraphQL to test and write GraphQL queries in the context of Gatsby, using the GraphQL web-based application. We also learned how GraphQL obtains Sanity's content through its GraphQL API.

In the next chapter, we'll learn about how Gatsby can source additional content from sources such as the filesystem and **Content Management Systems** (CMS), as we move closer to building a website using Gatsby.