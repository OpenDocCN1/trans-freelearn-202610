![](bbox=[0.18, 0.1, 0.82, 0.36])

Figure 5.6 – An illustration of a Gaboon viper on the forest floor

Let's create some data on our snake population movements with Script 5.1:

```
#create snake area network adjacency matrix
import numpy as np
import networkx as nx
np.random.seed(0)

adj_mat = [[1,1,1,0,0,0,1],
           [1,1,1,0,0,0,0],
           [1,1,1,0,0,0,0],
           [0,0,0,1,1,1,0],
           [0,0,0,1,1,1,0],
           [0,0,0,1,1,1,1],
           [1,0,0,0,0,1,1]]

adj_mat = np.array(adj_mat)
```

In practice, geographic regions rarely share borders with all other regions in a dataset. **Spatial weighting** encodes **regional connectivity**. Geographically isolated areas are not likely to contain overlapping snake populations. However, it is possible that snakes will migrate across geographically isolated regions, and spatial weighting might not be appropriate in some animal population studies; however, Loango National Park does not include major barriers to travel like some mountainous or island geographies. Here, we’ll add to Script 5.1 spatial weighting component (weights total) to our adjacency matrix of snake overlap (adj_mat) to derive a weighted adjacency matrix (adj_mat_w):

```
#create spatial weight matrix
weights_total = [[1,1,1,0,0,0,1],
```