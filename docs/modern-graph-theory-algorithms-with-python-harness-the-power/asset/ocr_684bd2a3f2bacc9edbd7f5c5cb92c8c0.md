# bnlearn Directed Acyclic Graph (DAG)

![](bbox=[0.1, 0.13, 0.85, 0.52])

Figure 10.9 – A plot of course pass dependencies among the three courses simulated to have interdependencies

Note that while the exhaustive search finds the correct causal pathway, it is impractical for most real-world problems, as its run time limits are used on datasets with more variables. Even for our full dataset, the runtime to compute a Bayesian network with exhaustive search is impractical. However, the greedy hill-climbing algorithm was good enough to parse through our data and find likely relationships that exist in the dataset. In the real world, mining for pathways is usually just a first step in understanding a system. Identifying the main components usually suffices for the next steps, which we’ll discuss next.

# Structural equation models

Once we have an idea of which parts of a pathway may lead to an outcome of interest, we can form a hypothesis regarding the logical steps between these parts. For instance, in our course example, we may know that most students take Course_1 before Course_2 and Course_2 before Course_3. This leads to a hypothesis that Course_1 impacts performance in Course_2 and Course_3 and that Course_2 impacts performance in Course_3. All three courses are assumed to influence performance in the licensing exam. This gives us the hypothesized pathway shown in Figure 10.10: