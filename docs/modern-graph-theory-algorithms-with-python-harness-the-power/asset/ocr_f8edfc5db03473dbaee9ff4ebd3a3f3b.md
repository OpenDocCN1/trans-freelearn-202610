```
```python
# create market graph and get rid of loops created from the
#correlation
bf_market_w=Graph.Adjacency(cor_weighted,mode="undirected")
edge_list=bf_market_w.get_edgelist()
self_loop=[]
for i in range(0,46):
    self=(i,i)
    self_loop.append(self)
to_remove=[]
for i in edge_list:
    for j in self_loop:
        if i==j:
            to_remove.append(i)
bf_market_w.delete_edges(to_remove)

#create plot
ig.plot(bf_market_w)
```
```

This should yield many more isolated markets that are not highly correlated over our time period:

![](bbox=[0.271, 0.463, 0.742, 0.838])

Figure 2.10 – A plot of Burkina Faso markets connected by local
Moran statistics over the selected threshold of 0.9