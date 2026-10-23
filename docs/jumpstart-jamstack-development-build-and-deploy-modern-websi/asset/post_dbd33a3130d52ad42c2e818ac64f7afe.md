
## public

Located outside of the `src` directory, this folder is where the deployed website pages will be created if they do not already exist when `gatsby build` is run in the command line. This is also where the files will be referenced as the document root when a Gatsby website is first viewed using the `gatsby serve` command. The contents of the public directory may be deleted and recreated at any time since running `gatsby build` will always recreate them.

The complete Gatsby documentation may be found at https://www.gatsbyjs.com/docs/, and looks as follows:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_109_0.png)

Figure 7.1 – Gatsby website documentation

Now that we've learned about Gatsby's file and folder structure, we will move on to learn how to use Gatsby. To start up Gatsby, we'll need to use the command line. Start Gatsby in development mode.

## The gatsby develop command

Using Gatsby at the command line provides many different types of functionality for the various project stages. Initially, the `gatsby develop` command will start up Gatsby in development mode. This allows changes to be viewed in real time. The command won't stop running until you press `Ctrl + C`. This key combination is used in the UNIX operating system to terminate (kill) a process.

From inside the /web directory, type gatsby develop, as this will cause the Gatsby development environment (.env.development) to run, using the development configuration, as opposed to the production configuration (.env.production). Also, it will create a GraphQL playground called **GraphiQL** (pronounced like *graphical*). This will allow us to actually interact with Sanity's GraphQL interface and to begin to write, test, and view the results of the queries needed to create our events website.

## Summary

In this chapter, we learned about what Gatsby is and how it is built using JavaScript and React. Next, we learned how its configuration file is created, and what its main parts are. We also learned how to configure the gatsby-source plugin to interact with Sanity. We also learned about the gatsby develop command and how to start up a project.

In the next chapter, we'll begin our journey with GraphiQL, a GraphQL playground, and move closer to creating a website. This will allow us to design, execute, and test GraphQL queries within the Gatsby context.

## 8
Gatsby and GraphQL

In this chapter, we’ll begin to bring content into Gatsby through GraphQL. GraphQL is a good starting point for learning about how Gatsby works, since Gatsby sources content using GraphQL. As we learned in Chapter 6, Sanity’s GraphQL Playground, Sanity exposes its information through a GraphQL Application Programming Interface (API), so Gatsby can easily use Sanity’s content through GraphQL.

These are the main topics that we will cover in this chapter:

- GraphQL in GatsbyJS
- GraphiQL, a GraphQL navigator

## Technical requirements

For this chapter, the Gatsby application needs to be installed and a Terminal program is needed. Additionally, a web browser is needed.

## GraphQL in GatsbyJS

In this section, we'll learn about how GraphQL is used in the context of Gatsby. We will learn what the major difference is between how GraphQL is used by Gatsby and how it is exposed by Sanity's GraphQL API.

Gatsby's use of GraphQL is different from Sanity's since it uses the concept of a graph to formulate its GraphQL queries. Let's learn what a graph is.

In the following diagram, the figure consists of nodes and edges:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_112_0.png)

Figure 8.1 – A graph

In Gatsby's implementation of GraphQL, in reference to the graph, each Sanity document is represented as a node. Recalling the GraphQL query that was used in Chapter 6, Sanity's GraphQL Playground, to return all events, this query was quite simple, as highlighted in the following code snippet:

```
{
  allEvent {
    name
  }
}
```

In Gatsby, we find that the corresponding query to return all events has two main differences, as follows:

- allSanityEvent is used in place of allEvent. Sanity's developers created and officially maintain the gatsby-source-sanity plugin. They have chosen to add this prefix to differentiate Sanity documents from others, a technique sometimes called namespacing. Gatsby endorses this as a good practice. Obviously, using GraphQL Playground on the Sanity side means that only Sanity's content is being sourced, whereas in Gatsby it is possible to use many different content sources at the same time.

- The query needs to specify the edges and node parts to be able to return each event, as shown in the following code example:

```
query MyQuery {
  allSanityEvent {
    edges {
      node {
        name
      }
    }
  }
}
```

Likewise, since GraphQL is specifying not only the request format but also the result format, results are returned encapsulated in an array of edges and a node for each document, as follows:

```
{
  "data": {
    "allSanityEvent": {
```

Likewise, the results of the query also contain the edges and nodes, as shown in the following code snippet:

```
{
  "data": {
    "allSanityEvent": {
      "edges": [
        {
          "node": {
            "name": "The Appleseed Cast"
          }
        },
        {
          "node": {
            "name": "Friday Drinks"
          }
        }
      ],
    }
  }
```

![](img/97ab031f38e2174e2f1b51da18dfa1d0_115_0.png)

# Figure 8.2 – GraphiQL

On the left-hand side, we can see the GraphQL Explorer. This helps us to automatically create GraphQL queries by providing the user with a view of all of schemas available for use. So, by expanding each of the schemas, options for filtering and sorting are shown, along with the fields (including their nest elements) that are available to be returned as the query result. Let's look, in detail, at an example similar to what was previously discussed in Chapter 6, Sanity's GraphQL Playground.

Clicking on the desired starting query—for example, allSanityEvent—the various entity fields that may be used for filter options will be seen, and the basic query will be created in the middle section. As the various fields are chosen, GraphiQL will automatically add those fields to the graphql query.

Next, let's examine the following screenshot of the expanded allSanityEvent section:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_116_0.png)

Figure 8.3 – GraphiQL with an example query

As the allSanityEvent section is expanded, followed by filter, the filter options are shown. virtual eq false filters results based on the following criteria: only the events that are not virtual (eq false) are selected. The following screenshot shows details of the left-side navigation that allows for the filtering and selection of fields to return from the query:

![](img/97ab031f38e2174e2f1b51da18dfa1d0_117_0.png)

Figure 8.4 – Query options

Then, name is chosen from the list of node fields after expanding edges.

The query is automatically created, as follows:

```
```graphql
query MyQuery {
  allSanityEvent(filter: {virtual: {eq: false}}) {
    edges {
      node {
        name
      }
    }
  }
}
```
```

To execute the query, click on the **Play** button.

The results are shown in the third panel, on the right-hand side. Two other features are **History** and **Prettify**. **Prettify** is used to properly format the code.

```
```
name
}
}
}
}
~export default ComponentName
```
```

Let's examine the code that is generated. The first line imports React. The second line imports graphql from gatsby, needed to perform the query. Next, we define the component called ComponentName, by creating an anonymous function and assigning it to a constant, since the function will not change. In this case, ComponentName is a valid name but is meant to be a placeholder that should be changed.

Next, let's examine what a single event query would look like. Instead of allSanityEvent, which would return all events from the Sanity side to Gatsby, we will use the single sanityEvent event query for GraphQL, which will return just one record, accepting the id of the event from the Sanity side, denoted by an underscore. The following code snippet shows an example of this concept:

```
```
query MyQuery {
 sanityEvent(_id: {eq: "6ddd2730-9aca-46dd-a7fd-850ab306f7fb"}) {
 name
 }
}
```
```

Next, let's examine the results. Notice in the following code snippet that the results are wrapped in a data object:

```
```
{
 "data": {
 "sanityEvent": {
 "name": "IT Conference"
 }
 }
}
```
```
