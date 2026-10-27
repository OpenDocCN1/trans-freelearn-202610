If we open reachability-meta.csv and take a look at the first few lines, we’ll find a list of information about cities in the US and Canada:

```
"node_id","name","metro_pop","latitude","longitude"
0,"Abbotsford, BC",133497.0,49.051575,-122.328849
1,"Aberdeen, SD",40878.0,45.45909,-98.487324
2,"Abilene, TX",166416.0,32.449175,-99.741424
3,"Akron/Canton, OH",701456.0,40.79781,-81.371567
```

The file contains node attributes for our city nodes, keyed by the node ID. We have city names, the cities’ populations, and their respective latitudes and longitudes. This CSV file can be loaded into Neo4j as it is, so we don’t need to do any preprocessing.

However, our other file, reachability_250.txt, is not a CSV, which Neo4j prefers for imports. Opening reachability_250.txt and examining the first new lines, we can see that this contains a list of edges:

```
# Directed graph: reachability.txt
# Transportation reachability of cities in the United States and Canada
# (edge from i to j if the estimated travel time from i to j is less than a threshold)
# Edges are weighted by similarity (more negative --> more dissimilar)
# Nodes: 456 Edges: 27677
# FromNodeId ToNodeId Weight
57 0 -84
113 0 -90
235 0 -170
```

According to the data source, the graph is directed. Each edge represents travel between two cities, represented by node IDs. The edge weighting is a measure of travel time between the two cities, where more negative values refer to further distances. There are also six lines of information that we will need to remove.

In the next steps, we will look at starting to clean and preprocess this data so that it is graph ready.

To preprocess this information ready for Neo4j import, let’s convert it to CSV format and remove the header lines. We can also make the distance measures non-negative to simplify the weightings. All of this preprocessing can be handled with the Python csv module and some list comprehensions, as we have done in previous chapters:

```
import csv
with open('./data/reachability_250.txt', 'r') as txt:
    reader = csv.reader(txt, delimiter=' ')
    edges = [edge for edge in reader][6:]
    edges = [[edge[0], edge[1], int(edge[2])*-1] for edge in edges]
```