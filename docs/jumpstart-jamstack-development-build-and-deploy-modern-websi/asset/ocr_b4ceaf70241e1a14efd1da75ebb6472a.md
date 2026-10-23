# The schema file

For Sanity to be able to recognize a new schema, that schema must be added to the schema.js file, which is in the studio/schemas folder. Let’s see how to do so:

+   1. Add the files via import statements at the top of the file, after the line starting with import siteSettings:

```
// document schemas
import siteSettings from './documents/siteSettings'
//insert these two rows
import event from './documents/event'
import venue from './documents/venue'
```

+   2. Add the imported schema to the list of schema types:

```
types: schemaTypes.concat([
// The following are document types which will appear
// in the studio.
siteSettings,
//insert two rows here
event,
venue,
...
])
```

# Deploying the schema

To view the schema changes, they must be deployed. When we deploy something, we move or copy it from one environment to another, usually a production environment where it can be used by other people and things. Here, we will push our changes to the schema into an environment where they may be viewed in the browser. In the root of the project directory, use either npm or yarn to install the dependencies.

Execute one of the following commands by typing it in the Terminal and pressing Enter:

```
yarn install
```

or

```
npm install
```