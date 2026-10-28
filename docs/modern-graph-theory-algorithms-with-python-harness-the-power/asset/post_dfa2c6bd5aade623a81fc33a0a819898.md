
waves). The R₀ value for COVID-19 varied dramatically between variants and countries, making it difficult to obtain a standard R₀. However, one estimate across Africa puts COVID-19’s estimated R₀ at 2.0-9.7 for most countries.

SIR models work a bit differently on networks. An infected vertex typically infects only neighbors in the network, creating a proximity effect. In our network, the bridging properties limit the chance of spreading from one subnetwork to another, as infection rates of a bridge vertex and its adjacent vertices are lower than spread through a hub of mutually connected vertices. In general, bridges are important vertices in networks with respect to information, ideas, or epidemic spread and are often intervention targets. Vaccination campaigns in a disease epidemic to limit spread, targeting in a marketing campaign to increase product adoption rates, and capturing in a criminal communications network are all real-world examples of bridge targeting to impact spread dynamics on a network of interest.

Let’s dive a bit deeper into the geometry of networks, including hubs and bridges. Hubs are regions of high connectivity, where many vertices connect to many other vertices. Geometrically, densely connected regions such as hubs have high curvature and provide many possible paths for spreading processes or random walks on the network. In our Francophone Africa network, Cameroon, Senegal, and Mali all show hub properties.

Note, however, that Senegal has many edges to other vertices that do not have large numbers of edges associated with them. While Senegal serves as a hub owing to its connectivity, it does not make an ideal spreading target, as the process will die in the fairly isolated vertices surrounding it. Cameroon and Mali, however, are hubs connected to other vertices that serve as either hubs or bridges, allowing the process to continue spreading with high probability. Centrality metrics measure how central a vertex is to the network, creating different types of network hub measurements that can find vertices that function as Mali/Cameroon or as Senegal within the network. We will revisit these centrality measurements in later chapters, where we will assess network properties with igraph.

Bridges work a bit differently but also contribute important geometric features to networks. Niger and Chad serve as bridges in our Francophone Africa network, connecting hubs that share no other edges between them. Technically, a type of centrality called betweenness centrality measures this bridging property by computing the shortest paths between each vertex pair (representing the most direct communication route) and counting the number of shortest paths that include a given vertex. Those vertices with many of the shortest paths passing through them act as bridges between different parts of the network. Geometrically, bridges serve as crossing points from one part of a network to another with respect to flow. Our music spread from Gabon to West Africa would not be possible without bridges such as Chad and Niger connecting North and West Africa with Central Africa, as the trend must spread through those countries to leave Central Africa.

For our music spread across Francophone Africa, let’s consider a SIR model that begins with 5% of our countries listening to a new Rumba artist. We’ll say that our music trend has a 25% chance of spreading to a neighboring country (our beta parameter) and a 10% chance of fizzling out as a trend (our gamma parameter). This gives us R₀~2.5, which means our music trend should spread well initially.

```python
viz = DiffusionTrend(model, trends)
p = viz.plot(width=500, height=400)
show(p)
```

This should give you a plot similar to Figure 3.7, which shows the fraction of susceptible, infected, and recovered/removed countries in our music trend epidemic. Note that our peak expected number of countries influenced by this new Rumba artist is about a third of our countries, and the influence of this new artist lasts for over half the year. This is a fairly successful outcome for our new artist:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_76_0.png)

Figure 3.7 – Music trend spread across the Francophone country network

Now, let's modify our gamma parameter to equal 0.3, giving an R₀ of less than 1. We'd expect our trend to fizzle out very quickly with these dynamics. Modifying Script 3.1 under this new scenario yields a much different dynamic (Figure 3.8):

```python
# Model Configuration
import ndlib.models.ModelConfig as mc

config = mc.Configuration()
config.add_model_parameter('beta', 0.25)
config.add_model_parameter('gamma', 0.3)
config.add_model_parameter("fraction_infected", 0.05)
model.set_initial_status(config)

# Simulation
iterations = model.iteration_bunch(50)
trends = model.build_trends(iterations)

#visualize spread dynamics
from bokeh.io import output_notebook, show
from ndlib.viz.bokeh.DiffusionTrend import DiffusionTrend

viz = DiffusionTrend(model, trends)
p = viz.plot(width=500, height=400)
show(p)
```

![](img/71ff737206f7d80bb625b3c9aca9dc7f_77_0.png)

beta: 0.25, gamma: 0.3, fraction infected: 0.05, tp rate: 1

Figure 3.8 – Music spread across the Francophone country network under the new gamma parameter

*Figure 3.8* indicates that the new Rumba artist’s music does not spread much to neighboring countries and fizzles out soon after the release of the album. It’s unlikely that the artist will be known outside their home country and the ones who have heard the album will forget the songs quickly. This is a much different outcome for our artist than the original scenario.

## AIMS Cameroon student network epidemic model

To understand how influenza might spread through a social network, let’s return to our AIMS Cameroon student network. Recall that we used survey data about interactions to create connections between students. This allowed us to connect students who interacted regularly. However, consider the case where survey data about interactions did not exist. How might we infer possible connections between students?

We have information such as country of origin, field of study, and age of the students. Students from the same countries likely share a common culture—perhaps the same local language, the same favorite comfort foods, or the same favorite musicians.

Similarly, students in the same field of study are likely to spend time together in and out of class, given their shared courseload and career interests. Study groups, group projects, and professional events in their fields likely bring students together on a regular basis. Should an infectious outbreak such as influenza or COVID-19 happen at any of these gatherings, it will likely spread within fields of study.

Let’s create a new AIMS Cameroon student network by connecting students who come from the same country or share a field of study. We’ll load the dataset with `Script 3.2`:

```python
#import needed packages
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt
import numpy as np
import math

#reading the dataset
fichier = pd.read_csv("C:/users/njfar/OneDrive/Desktop/AIMS_data.csv")
data = pd.DataFrame(fichier)
```

Now, let’s set up our network and some preliminary vectors and quantities to help us generate connections between students, including the number of students and their attributes, by adding to `Script 3.2`:

```python
#initializing the social network
aimsNetwork = nx.Graph()

#populating the network with nodes and edges
for i in range(len(data["Name"])):
    aimsNetwork.add_node(
        data["Name"][i], Age=data["Age"][i],
        country=data["Country"][i], Field=data["Field"][i],
        background=data["Background"][i])

#define length
N = len(aimsNetwork.nodes())

# one can build the adjacency matrix
AIMS_by_Country_or_Field = nx.Graph()

#define objects
AIMS = aimsNetwork
students = list(AIMS.nodes())
```

Now, we can connect students according to shared home country or field of study by looping through our attributes to build a graph in NetworkX by adding to Script 3.2:

```python
#create edges
for i in range(N-1):
    for j in range(i+1,N):
        sti = students[i]
        stj = students[j]
        if (AIMS.nodes[sti]['Field'] == AIMS.nodes[stj]['Field'] or
            AIMS.nodes[sti]['country'] == AIMS.nodes[stj]['country']):
            AIMS_by_Country_or_Field.add_edge(sti,stj)
```

Now that we have our network, let’s visualize the connections between students at AIMS:

```python
#create plot
Degree=AIMS_by_Country_or_Field.degree()
var = [10*k[1] for k in list(Degree)]
plt.figure(figsize=(20,20))
nx.draw_random(AIMS_by_Country_or_Field, font_size=10,
    node_size = var, with_labels=True, node_color="red")
plt.show()
```
