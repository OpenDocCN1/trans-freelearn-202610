# Declarative data descriptions and validations with plumatic/schema

Clojure is dynamically typed language and often provides productivity when you develop alone or in a small team. However, developments in larger teams sometimes require knowledge about data between members. prismatic/schema provides the solution for the aforementioned problems.

In this recipe, we will learn how to add descriptive expressions and validations in Clojure. We will use prismatic/schema to define schemas for various data types and validate them with schemas. prismatic/schema can define functions that define schemas for input parameters and a return. We will then learn how to coerce one data type to another.

Finally, we will learn other solutions. We will introduce core/typed tries to provide a type inferences system similar to Scala. core/spec is a new feature and introduced in Clojure 1.9.

# Getting ready

Let's create a project using prismatic/schema. prismatic/schema, which supports both Clojure and ClojureScript. So, let's create a project using Figwheel:

```
$ lein new figwheel schema-example
$ cd schema-example
```

We will add the library dependency for prismatic/schema in the project.clj:

```
:dependencies [[org.clojure/clojure "1.8.0"]
                [org.clojure/clojurescript "1.8.51"]
                [org.clojure/core.async "0.2.374"
                 :exclusions [org.clojure/tools.reader]]
                [prismatic/schema "1.1.1"]]
```