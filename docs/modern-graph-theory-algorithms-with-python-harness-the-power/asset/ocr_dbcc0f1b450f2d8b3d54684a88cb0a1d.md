Figure 9.3 shows two separate groups, with one very small group consisting of two individuals and a much larger group with sparse and dense connectivity among individuals in the group. We’d expect the degree and PageRank centralities to vary quite a bit among individuals, given the connectivity patterns of our friendship dataset. Let’s add to Script 9.1 and append our feature matrix to rerun our k-means analysis, including both demographic factors and two scaled centrality metrics, to see how our clustering changes:

> Note
> You may find a warning about copying objects; this does not impact the analysis or object.

```
#create scaled metrics and attach to X
d=np.array(Graph.degree(friends))/10
p=np.array(Graph.pagerank(friends))*20
X['degree']=d
X['pagerank']=p

#create new k-means model with graph metrics added
km2=KMeans(n_clusters=3,init='random',n_init=5)
km_model2=km2.fit_predict(X)

#explore new k-means model
km_model2

#add to dataset as first solution
km_2=np.array(km_model2)+1
mydata['km_2']=km_2
```

We can see some changes in our clustering results compared to our initial k-means model. In cluster #0, individual 19 is added (a teammate who does homework with the initial #0 cluster and attends the game). Our initial cluster #1 shows individuals 8-9, 12, 16, and 23-25; individuals 13, 14, and 19 are no longer assigned to this cluster but other individuals remain. In the remaining cluster, individuals 13 and 14 join our initial cluster, both of whom seem to have more connectivity than initial cluster #1, fitting better with cluster #2 based on centrality metrics. It seems that adding network connectivity metrics improves k-means clustering results, as individuals who may not share every activity but show similar group connections are reassigned to groups that more closely fit their positions within the social network.

Let’s now see how we can use a semi-supervised clustering algorithm that we first encountered in Chapter 5—spectral clustering—to obtain a semi-supervised solution to our friendship network clustering.