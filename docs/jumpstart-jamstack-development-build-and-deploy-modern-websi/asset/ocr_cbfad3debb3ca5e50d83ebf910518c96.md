This query will return the following result:

```
```
{
  "data": {
    "allNodeArticle": {
      "edges": [
        {
          "node": {...}
        }
      ],
    "allMarkdownRemark": {
      "edges": [
        {
          "node": {...}
        }
      ]
    }
  }
}
```
```

As we can see, we have both the Drupal and filesystem content from the two different sources, in the same query result.

# ## Summary

In this chapter, we learned how to extend Gatsby using plugins to enrich the functionality of the application. We learned about the following: what a plugin is; npm; how to use semantic versioning; and how to install and update a plugin. We also learned about the structure of the Gatsby Plugin Library and how to install and configure a Gatsby plugin. We installed and configured the `gatsby-source-filesystem` and `gatsby-transform-remark` plugins, and we used GraphQL to query content from a Markdown file. We also installed and configured the `gatsby-source-drupal` plugin and used GraphQL to query content from a Drupal application.

These plugins are useful when we need to get content from other sources, especially if we need to build an application that aggregates content from different sources.

In the next chapter, we are going to talk about Gatsby components and how pages, templates, and partials are structured, and how they differ.