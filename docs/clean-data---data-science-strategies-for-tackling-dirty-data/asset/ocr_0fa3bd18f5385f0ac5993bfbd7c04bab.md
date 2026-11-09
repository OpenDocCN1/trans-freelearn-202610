Here is an excerpt of what a nodes section looks like:

```
nodedef>name VARCHAR,label VARCHAR,sex VARCHAR,locale VARCHAR,agerank INT
1234,Bugs Bunny,male,en_US,296
2345,Daffy Duck,male,en_US,295
3456,Minnie Mouse,female,en_US,294
```

Here is an excerpt of what an edges section looks like. It shows that Bugs (1234) and Daffy (2345) are friends, and Bugs is also friends with Minnie (3456):

```
edgedef>node1 VARCHAR,node2 VARCHAR
1234,2345
1234,3456
3456,9876
```

# Step three – convert the GDF file into JSON

The task we want to perform is to build a representation of this data as a social network in D3. First, we need to look at the dozens of available examples of D3 to build a social network, such as those available in the D3 galleries of examples, https://github.com/mbostock/d3/wiki/Gallery and http://christophevi au.com/d3list/.

These examples of social network diagrams rely on JSON files. Each JSON file shows nodes and the edges between them. Here is an example of what one of these JSON files should look like:

```
{"nodes": [
  {"name":"Bugs Bunny"},
  {"name":"Daffy Duck"},
  {"name":"Minnie Mouse"}],
"edges": [
  {"source": 0,"target": 2},
  {"source": 1,"target": 3},
  {"source": 2,"target": 3}]}
```

The most important thing about this JSON code is to note that it has the same two main chunks as the GDF file did: nodes and edges. The nodes are simply the person's name. The edges are a list of number pairs representing friendship relations. Instead of using the Facebook identification number, though, these pairs use an index for each item in the nodes list, starting with 0.