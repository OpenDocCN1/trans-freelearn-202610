For more technically minded readers, we can arrange these terms to define a system of differential equations governing spread and recovery within a population of susceptible individuals, as shown next:

\(\frac{dS}{dt} = -\frac{\beta IS}{N}\)
\(\frac{dI}{dt} = \frac{\beta IS}{N} - \gamma I\)
\(\frac{dR}{dt} = \gamma I\)

A high transmission rate coupled with a low recovery rate will yield a large population of infected individuals. A low transmission rate with a high recovery rate will result in a slow or fizzling-out epidemic in the population.

However, interactions within a population are rarely random, as assumed by the SIR equations. Dynamic networks provide a way to capture interactions over time, leading to more accurate models of transmission within a population of interest or allowing for more accurate planning through simulated models of interaction levels within a population of interest. Let’s build on Chapter 3’s discussion of epidemics by introducing some new network metrics that influence epidemic spread.

# Factors influencing spread

In Chapter 3, we examined the importance of bridges and hubs to the spread of a music trend or epidemic across networks. Since then, we’ve considered many network centrality metrics that relate to the concepts of hubs and bridges. Let’s briefly review a few of these centrality metrics.

Hubness can be assessed with several centrality metrics, including the following:

+   - Degree centrality (defined in Chapter 6)
+   - PageRank centrality (defined in Chapter 6)
+   - Hubness centrality (defined in Chapter 6)

Degree centrality is the simplest and most related to spreading processes on a network. The number of connections an infected vertex has to other vertices impacts the ability of an infected vertex to spread the disease to its immediate neighbors. The combination of degree centralities across the network impacts the overall ability of a disease to spread globally. Sparse networks with very low degree centralities don’t offer chances for the disease to infect many individuals at the same time, leading to a situation where public health workers can effectively implement strategies such as isolation and contact tracing without being overwhelmed by many infected patients at a given time point.