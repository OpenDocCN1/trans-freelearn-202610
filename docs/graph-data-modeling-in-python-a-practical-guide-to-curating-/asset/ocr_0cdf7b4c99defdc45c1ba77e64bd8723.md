+   2. Click **Start** on the new database to get it running, and the example database will be stopped, as Neo4j only supports one concurrent database running in the Desktop edition. The example database can now be deleted.
+   
+   3. The Desktop edition of Neo4j comes bundled with the Neo4j Browser. Click **Open** next to our newly created database to open a Neo4j Browser window. Primarily, we will be focusing on using Neo4j for its function as a database, over its ability to visually display networks. From here, we can also create a new user, whose credentials we will use to access the database from Python. In the command line at the top of the window, enter the following:
+   
+   4. A window containing fields to complete for a new user will appear. In the username field, add admin. For the password, again, we will use testpython. For roles, select both **admin** and **PUBLIC** from the drop-down menu, so that the new user will have full access to the database, both read and write.
+   
+   5. Finally, click **Add User** to create our new admin user.

```
:server user add
```

Now, with Neo4j installed and our Neo4j database created and configured, we will next need to learn how to interact with it, through the Cypher query language.

# The Cypher query language

Cypher is an open source query language compatible with several graph databases, including Neo4j. It shares some similarities with more common query languages, such as SQL, but is designed with graphs in mind, as opposed to tabular data. Much of Cypher's design is centered around matching node and edge patterns in a graph, using a special syntax that almost looks like drawing network paths.

When we want to refer to a node using Cypher, we enclose it in parentheses. Similarly, to denote a specific edge, square brackets are used. Nodes and edges are linked with dashes and arrows, like so:

```
(node) - [edge] -> (node)
```

This pattern would match nodes in a graph that are connected to other nodes, in a directed fashion. In practice, we are likely to be much more specific about the nodes we create or return.

We can begin by executing some simple Cypher queries in the Neo4j Browser window, before we start to query Neo4j directly from Python. Let's start by creating some nodes:

+   1. The CREATE function can be used at the start of a line to instruct Neo4j to create new nodes. To create a new node of the Person type with the name Jeremy, we open parentheses and use a colon to designate the type of the node. This is followed by opening curly braces to add the name attribute to this node, with the value Jeremy. We can follow this pattern to create two Person nodes:

```
CREATE (:Person {name: "Jeremy"})
CREATE (:Person {name: "Mark"})
```