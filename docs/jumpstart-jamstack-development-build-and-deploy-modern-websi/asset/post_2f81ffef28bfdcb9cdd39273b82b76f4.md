
## Gatsby page components

One of the simplest types of components used in Gatsby is a page component. Gatsby page components are used only once for single, unique pages, such as the index (home) page, 404 page, or an about us page, for example. This page will only be generated once.

### Building and modifying Gatsby page components

As mentioned previously, pages are unique. By creating a page component and placing it in the `/web/src/pages` folder, Gatsby will automatically generate the page.

The content of a page component in Gatsby can be as simple as the following example, which contains the entire page contents inside a string:

```
// Does not require React because there is no JSX
const AboutUsPage = () => (
  `Who we are: .......`
)
export default AboutUsPage
```

The following is a shorter way to write the preceding code:

```
const AboutUsPage = () => {
return `Who we are: .......`
}
```

Enhancing this example by using JSX would allow us to create the following example:

```
//Requires React because this example uses JSX tags.
import React from 'react'

const AboutUsPage = () => (
  <html>
    <body>
      <h1>Who we are:</h1>
        <p>.......</p>
    </body>
  </html>
)
export default AboutUsPage
```

If we were to name the file `about-us.js` and place it in the `/web/src/pages/` directory, then running Gatsby would display the following screenshot:

Who we are: ......

Figure 10.1 – Screenshot of the About Us page

This AboutUsPage example will be rendered in the browser as HTML.

## Gatsby template components

Gatsby template components are, as the name implies, templates. These are used as a template for pages, such as articles, where certain parts of the page will be filled with unique content. These templates will be processed as many times as necessary to produce all the content needed.

### Building and modifying template components

To illustrate this concept, we will create a template for the events on our website. The first file that we need to work with is `/web/src/gatsby-node.js`. This file contains the code used to display blog posts using a template.

At the bottom of the `gatsby-node.js` file, the relevant code is as follows:

```
exports.createPages = async ({graphql, actions}) => {
  await createBlogPostPages(graphql, actions)
}
```

We will now add another function to generate events sourced from Sanity through GraphQL:

```
exports.createPages = async ({graphql, actions}) => {
  await createBlogPostPages(graphql, actions)
  await createEventPages(graphql, actions)
}
```

Next, we'll copy the content of createBlogPostPages. The entire function should be copied. However, the middle of the function was omitted for space. Here is an excerpt:

```
async function createBlogPostPages (graphql, actions) {
  const {createPage} = actions
  const result = await graphql(`
  .....
createPage({
    path,
    component:
        require.resolve('./src/templates/blog-post.js'),
    context: {id}
    })
})
```

Here is the structure of the newly created createEventPages function:

```
async function createEventPages(graphql, actions) {
    const {createPage} = actions
    const result = await graphql(`
    {
        allSanityEvent {
            ...
        }
    }
`)
    ...
    if (result.errors) throw result.errors

    const eventEdges = (result.data.allSanityEvent || {}).edges || []
    ...
    eventEdges
    .forEach((edge, index) => {
        ...
    })
```

Now, let's examine this code. An asynchronous function is created with the graphql and actions parameters. This will always return a promise:

```
async function createEventPages(graphql, actions) {
```

The createPage method is extracted as a constant, or destructured from actions:

```
const {createPage} = actions
```

The constant result is declared to hold the result of the GraphQL query. The JavaScript function will wait for this to complete before moving on. The query is enclosed in backtick operators (`):

```
const result = await graphql(`
{
```

The following lines are similar to the GraphQL query, which returns Gatsby's id and `dateAndTime` from `allSanityEvent` via `gatsby-source-sanity`:

```
allSanityEvent {
  edges {
    node {
      id
      dateAndTime
    }
  }
}
`
```

If there are any errors, throw an exception:

```
if (result.errors) throw result.errors
```

Next, we'll examine the following line of code:

```
const eventEdges = (result.data.allSanityEvent || {}).edges || []
```

Declare a constant named `eventEdges` and assign the results of the query's edges to it if they are not empty. Otherwise, if there are no results, then instantiate an empty array to `eventEdges`. Remember that the result of the GraphQL query is wrapped in data.

Iterate, or loop through (using forEach) all of the edges, using an edge variable for each single edge of the GraphQL query:

```
eventEdges
.forEach((edge, index) => {
```

Destructure `id` and `dateAndTime` from the edge's node object:

```
const {id, dateAndTime} = edge.node
```

Use the `format` function from `date-fns`, which is included in the Gatsby `package.json` file to format `dateAndTime`, which otherwise would print as `2020-04-10T15:30:00.000Z`, into a more acceptable four-digit year, forward slash, double-digit numeric month format, (YYYY/MM), assigning it to a constant named `dateSegment`:

```
const { format } = require('date-fns')
const dateSegment = format(dateAndTime, 'YYYY/MM')
```

Create the path that will actually be the single page's URL and be seen in the browser. Here, we are using Gatsby's ID, which is created, and not Sanity's ID, which can be accessed in the GraphQL query as `_id`:

```
const path = `/event/${dateSegment}/${id}/`
```

Here is an example of the URL path:

```
/event/2021/09/dd88fc6c-10c5-572e-8c58-696add54a14a/
```

Finally, the `createPage` function is called, passing in the path, specifying the `event.js` template file for each page, and passing in `id` as the `context` parameter:

```
createPage({
  path,
  component:
    require.resolve('./src/templates/event.js'),
  context: {id}
})
```

The contents of the `event.js` file will be used in the `component` attribute of the `createPage` method and will be resolved, combining the contents of the page and its template to produce a single page.

### Building the event.js template

`event.js` is the template used for each individual event page, so let's look at how it is constructed:

1. First, we need a few import statements to bring in the necessary features:

```
import React from 'react'
import {graphql} from 'gatsby'
import Container from '../components/container'
import GraphQLErrorList from '../components/graphql-error-list'
import Event from '../components/event'
import SEO from '../components/seo'
import Layout from '../containers/layout'
import {toPlainText} from '../lib/helpers'
```

2. Next, we need to perform a GraphQL query to obtain each element of the page:

```
export const query = graphql`
  query EventTemplateQuery($id: String!) {
    event: sanityEvent(id: {eq: $id}) {
      name
      _rawBody
      venue {
        name
      }
    }
  }
`
```

At a minimum, to produce the event's page, we need its name, `_rawBody`, and the name of the event's related venue.

3. Next, `EventTemplate` is defined with props (properties) as its parameter and eventually exported:

```
const EventTemplate = props => {
...
}

export default EventTemplate
```

4. Next, the data and errors (if present) objects are destructured from the props parameter. Then, if data is present, the `event` constant is instantiated to hold the data object's event:

```
const {data, errors} = props
const event = data && data.event
```

5. Next, the `Layout` component is called inside the `SEO` component and the `Event` component. The error handling code has been removed to remove any distraction, but errors would be displayed here as well if they existed. The layout produces the page layout, tags, such as the header and footer, and provides a wrapper for the event:

```
return (
  <Layout>
    {event && <SEO title={event.name || 'Untitled'} />}
    {event && <Event {...event} />}
  </Layout>
)
```

> Note
> Notice how the JavaScript ES6 spread syntax is used to assign each of the Event object's values to separate values to be used with the Event component.
