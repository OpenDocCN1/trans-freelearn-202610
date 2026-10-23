Our query will contain calls and fields belonging to both plugins. We will get one single response, containing results from both sources separated by their root name. In this case, the Drupal content will be contained under the `allNodeArticle` array, and the filesystem content will be contained under the `AllMarkdownRemark` array.

As we already seen, we can go to `http://localhost:8000/___graphql` and run the following query:

```
query MyQuery {
  allNodeArticle {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          title
          date
          excerpt
        }
        html
      }
    }
  }
}
```