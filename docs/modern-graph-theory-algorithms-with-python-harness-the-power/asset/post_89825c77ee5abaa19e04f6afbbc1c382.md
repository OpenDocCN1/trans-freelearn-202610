
## Introduction to graph theory and networks

Social connections are fundamental to society, including family ties, shared roles in communities, trading relationships, and many more. Social networks—from MySpace to Twitter to TikTok—have played a larger and larger role in marketing, job-hunting, and information sharing in the last decades. Software systems might even be built between engineers scattered across the globe collaborating on platforms such as GitHub or Slack to coordinate efforts from teams in Kenya, India, and Australia who have never met.

A branch of data science called **network science** studies these relationships between individuals, groups, and even societies within social networks using algorithms and statistical methods originating in a field of math called **graph theory**. Graph theory studies pairwise relationships between objects. Graph objects (including people, towns, ideas, points in time, and many more types of objects to study) are represented in network science as vertices, or points, within the network; note that some disciplines may use the term “node” instead of “vertex.” Relationships between objects (such as mutual collaborations between people or bridges connecting different islands or statistical connections between time points) are represented as edges within the network and connect pairs of vertices that share a relationship.

To make this a little more concrete, let’s consider three young women: Ayanda in South Africa, Machiko in Japan, and Greta in Belgium:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_24_0.png)

![](img/71ff737206f7d80bb625b3c9aca9dc7f_24_1.png)

![](img/71ff737206f7d80bb625b3c9aca9dc7f_24_2.png)

Figure 1.1 – Ayanda, Machiko, and Greta

Perhaps Ayanda, Machiko, and Greta join an online women’s coding hackathon aimed at sustainable energy solutions. Through the hackathon, they meet and form a team to work on solar power solutions for rural villages. They’ve formed a professional network with mutual relationships among the three women, as shown in Figure 1.2:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_25_0.png)

Figure 1.2 – A network representation of Ayanda, Machiko, and Greta showing pairwise connections between the women

As these women work on their hackathon project, Ayanda may invite colleagues within her social network, adding to the team and creating new connections for Machiko and Greta over time. Perhaps Amara, a colleague of Ayanda’s from Kenya, connects with Greta but not Machiko to code the backend of their project, adding another member to the network:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_25_1.png)

Figure 1.3 – Social network with the addition of Amara

## What is a Network?

The evolution of networks over time is a key problem in network science today, as we often have incomplete data on relationships that exist outside of what we explicitly observe and want to predict future phenomena, such as predicting the potential for bipartisan fake news spread in the next election or when new members may join a criminal organization based on ties that exist in the present. We will wrangle some problems related to dynamic social networks and their applications in *Chapter 8*, where we analyze the spread of disease over time as a social network evolves.

Before jumping into real-world problems and their solutions in Python, it’s important to understand the fundamentals of network science. Let’s dive into some of the terminology and conventions of graph theory and networks.

### Formal definitions

Real-world networks are usually formalized and studied with tools from graph theory. As we’ve mentioned, graph theory defines relationships between objects in the form of vertices and the edges that connect related vertices. Let’s formalize this a bit and explore the foundations of network science found in graph theory. A graph is defined as a set of vertices and a set of lines, or edges, that connect pairs of vertices (Berge, 2001). Thus, a graph represents the structure of a network well. Returning to our hackathon social network, we represent our colleagues as vertices and connect them with edges if they collaborate.

Graphs can represent different types of relationships. A self-relationship of a vertex can be shown with a loop connecting the vertex to itself. For instance, when writing emails, someone might copy themselves on the email or send an email to themselves as a reminder. Another type of relationship is directionality. For instance, reposting content is a unidirectional interaction (usually), but friending someone is a bidirectional interaction where both parties mutually connect. Unidirectional relationships are represented in a graph with directed edges, usually denoted with an arrow on visualizations of the graph. Bidirectional relationships have undirected edges, denoted as lines without arrows. *Figure 1.4* shows a graph with loops, directed edges, and undirected edges:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_26_0.png)

Figure 1.4 — A graph with loops, directed edges, and undirected edges

In scientific literature, the terms network and graph are used interchangeably, as are terms for edges and vertices. Table 1.1 shows a few of the common terms encountered:

| Network Science | Graph Theory |
|---|---|
| Network | Graph |
| Node | Vertex |
| Link | Edge |
| Collection | Set |
| Relationship | Function |
| Directed graph | Digraph |

Table 1.1 – A terminology comparison between network science and graph theory

In practice, these distinctions are rarely made, so these two terminologies are often synonyms of each other. In this book, we’ll stick with edges and vertices as our graph components and networks as our preferred term for the collection of vertices and edges created from real-world data.

Many types of graphs exist within graph theory that are realized in real-world data. Let’s briefly define some types of graphs that exist:

- A directed graph contains one or more edges with a direction (to or from another vertex, called arcs)
- An undirected graph only has edges, which do not have an origin or destination vertex

“A graph is simple if it has no multiple lines (typically used to represent multiple relationships that exist or denote the strength of a relationship).” A simple undirected graph contains no loops, but a simple directed graph can contain loops. Let’s visualize what these graphs look like in practice:

- 2. We’ll then import igraph and its Graph module:

```
```python
#import igraph
import igraph as ig
from igraph import Graph
```
```

- 3. Next, we’ll use the Graph module to create a graph with three vertices with undirected edges connecting each vertex with the remaining vertices:

```
```python
#create hackathon network
g_colleagues=ig.Graph(
    edges=[(0,1),(0,2),(1,2)],n=3,directed=False)
```
```

- 4. Now, we can use the plot function to visualize our hackathon network. igraph offers many customization options, including vertex color, vertex size, edge color, edge size, labeling of vertices, and the size of the plot (among others). For this plot, we’ll bound the size of the image and create vertices large enough to label with the first initials of our hackathon network members:

```
```python
#plot the hackathon network
ig.plot(g_colleagues,bbox= (200,200), vertex_size=40,
    vertex_label=["M","A","G"])
```
```

This should give you a plot that looks like *Figure 1.6*, showing *Machiko*, *Greta*, and *Ayanda’s* hackathon network:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_29_0.png)

Figure 1.6 – A plot of the hackathon network in igraph

Now let’s repeat our network creation in NetworkX. NetworkX contains much of the functionality of igraph but provides easier integration with other network science tools and platforms, as well as simple ways to explore differential equations on networks:

- 1. Let’s first install NetworkX:

```
```bash
#install NetworkX
!pip install NetworkX
```
```
