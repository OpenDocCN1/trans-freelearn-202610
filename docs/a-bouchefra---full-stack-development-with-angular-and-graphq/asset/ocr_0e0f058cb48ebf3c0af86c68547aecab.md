Let's go over how to set up GraphQL Code Generator to generate typed documents and services for interacting with the GraphQL API step by step:

+   1. To begin, open your terminal and execute the following commands from the root of your monorepo project to install the necessary dependencies:

```
npm i @graphql-codegen/cli
npm i @graphql-typed-document-node/core
npm i @graphql-codegen/typescript
npm i @graphql-codegen/typescript-operations
npm i @graphql-codegen/typed-document-node
npm i @graphql-codegen/typescript-apollo-angular
npm i @graphql-codegen/import-types-preset
```

We previously installed the CLI while working on the server-side application, but reinstalling it ensures that we have the most recent version.

+   2. Open the codegen.yml file in the monorepo project's root directory and update it to point to your GraphQL documents using the documents field in the root-level configuration, as shown here:

```
documents: './packages/client/src/**/*.graphql'
```

The field was added at the same level as the schema field. The field accepts a variety of options. In our case, we used a glob expression to specify multiple files that could exist in the ./packages/client/src/ folder's subfolders. Check out https://www.graphql-code-generator.com/docs/getting-started/documents-field for more information on this field.

+   3. Under the generates field, add the following output for generating types:

```
generates:
  # [...] 
  ./packages/graphql-types/src/types.ts:
    plugins:
      - typescript
```

To generate the types that will be imported in the next generated files, we use the TypeScript plugin.