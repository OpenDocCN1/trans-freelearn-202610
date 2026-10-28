
## What is a Network?

+   2. Now, let’s import NetworkX:

```
```python
#import NetworkX
import networkx as nx
```
```

+   3. NetworkX works a bit differently than igraph in the construction of a network. We first define an empty graph, fill in the vertices, and then define what edges exist.

```
```python
#create hackathon network
G = nx.Graph()
G.add_nodes_from([1, 3])
G.add_edges_from([(1, 2), (1, 3), (2, 3)])
```
```

+   4. We’ll need to add attributes (our network member initials), import matplotlib, and then plot our graph:

```
```python
#plot the hackathon network
import matplotlib.pyplot as plt
G.nodes[1]['initial'] = 'M'
G.nodes[2]['initial'] = 'A'
G.nodes[3]['initial'] = 'G'
labels = nx.get_node_attributes(G, 'initial')
nx.draw(G, labels=labels, font_weight='bold')
```
```

This should show a plot similar in structure but a bit different in style to the one we constructed in igraph:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_30_0.png)

Figure 1.7 – A NetworkX plot of our hackathon network

We’ll be using both igraph and NetworkX in the coming chapters to show examples in both packages. For some problems, igraph will have better functionality; for others, NetworkX is preferable. In practice, you’ll encounter both as you implement network science on real-world problems. In the next sections, we’ll overview different network types and problems, laying the foundation for our future chapters.

## Random graphs

As network science began to tackle more and more real-world problems, understanding processes of network growth became an avenue of research:

*Was edge-building purely random?*

*Were there limits to the number of edges that any vertex might cultivate given the number of vertices?*

*Did vertices with many edges tend to encourage edges among unconnected vertices that shared an edge with a particular well-connected vertex?*

*Did vertices and edges separate into well-connected groups that didn’t connect well with other groups?*

A few growth process models gave rise to common types of growth patterns that are useful in network science research. Some of these, such as **Erdős-Renyi networks**, serve as comparisons for organized development in real-world networks. Erdős-Renyi networks are random networks, where edges between vertices are created probabilistically between any pair of vertices. For instance, to create a network with many edges, we might set the probability of vertex pair connections to 70%. To create a network with few edges, we might set the probability of vertex pair connections to 10%. Figure 1.8 (a) shows an Erdős-Renyi network with edge connection probability of 70%; Figure 1.8 (b) shows an Erdős-Renyi network with edge connection probability of 10%:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_31_0.png)

Figure 1.8 – Erdős-Renyi networks with edge probabilities of (a) 70% and (b) 10%

## What is a Network?

In the real world, preferential attachment, where some vertices attract connections to other vertices, is a more realistic model of how edges are added between vertices. One of the best-known examples of preferential attachment is the **Barabasi-Albert model**, where networks form more connections around certain vertices.

In a real-world example of preferential attachment, let’s consider a frontend engineer searching social media for backend engineers who are potential collaborators. Thus, the frontend engineer’s connections would not be random. They will be cultivated based on the engineer’s specific needs and the results of a search for backend engineers. **Scale-free networks** are models of this preference for attachment that often happens in real-world network formation processes. *Figure 1.9* shows the frontend engineer’s social network after adding the needed connections, who may not know each other before connecting with the frontend engineer:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_32_0.png)

Figure 1.9 – A scale-free network of engineering connections cultivated
by the frontend engineer’s search of social media

One of the major criticisms of scale-free models of network formation is the lack of hubs, or regions with high connectivity between a set of vertices and relatively few connections with other sets of vertices. **Watts-Strogatz models** account for this tendency to form denser regions of connections. As an example, gene networks often form clusters of related genes that influence each other functionally; some gene clusters also influence other gene clusters, forming occasional connections between gene sets with high connectivity, as shown in *Figure 1.10*:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_33_0.png)

Recent research has shown that none of these random network models capture all the nuances that come with real networks. Many real networks have aspects of preferential attachment and aspects of dense clustering, as well as some random connections that would not be expected given what is known about the network. Friends of friends often meet. Individuals find others who share their interests by searching social networks or connecting to a friend’s other social groups. Some friendships randomly start when individuals are stuck in an elevator or on a ferry.

Now that we have explored how to create networks in Python, let’s turn back to some real-world examples of networks, including directed and undirected networks and networks that change structure over time.

## Examples of real-world social networks

Real-world networks are often more complicated than the examples we’ve considered. Let’s consider some nuances in social media networks. In the simplest forms, a social network contains mutual connections between friends at a single point in time. However, some friends may interact more often on the social media platform than others; they may comment on posts, reshare content, and message each other frequently. Weighted networks assign numerical values to edges in a network according to some measurement of relationship strength or frequency of interactions.

Let’s return to our hackathon network and weight our edges based on the average number of conversations per day of the hackathon between different team members. Perhaps Machiko is coordinating the business use case of the technology and does not interact much with Ayanda, the backend engineer, but needs to make sure the front end is intuitive (Greta’s team position). However, we’d expect Ayanda and Greta to work together more frequently, as they are integrating the backend and front end of the product. Perhaps we have a weighted network as shown in Figure 1.11:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_34_0.png)

Figure 1.11 – A weighted hackathon network of conversations between team members

In addition to weighting, relationships can be undirected or directed. So far, we’ve considered undirected networks, where relationships are mutual. Friendships, Zoom calls, shared classes, team membership, and many other real-world networks involve mutual relationships. However, messages are one-way interactions, between a sender and receiver. Reposts of social media content are one-way relationships, as well. Gene regulation, population migration, airline flights, and goods transportation also involve one-way interactions. Directed graphs allow us to capture directional information in our graph, and like undirected graphs, directed graphs can be weighted or unweighted.

Let’s consider a football game where the players pass a ball to each other over a 5-minute period. Perhaps one forward dribbles for a while and then passes the ball to the other forward, who quickly passes it to the open center striker. Figure 1.12 shows this weighted network of football field interactions among the teammates:

Figure 1.12 – A network of football teammates passing to each other over a 5-minute period

Networks are rarely static. Consider a group of friends graduating from university. Some will stay in touch over the next several years; some will lose contact. The strength of lasting friendships may change, as well. Those who stay in the same area where they went to university might strengthen their friendships as they enter the workforce. Those who move away may connect with others in their new location and start losing touch with friends in other areas. Let’s consider four close friends graduating from university in Cape Town to pursue different careers in different cities:

Figure 1.13 – A friendship network of university graduates moving to different cities and pursuing different careers
