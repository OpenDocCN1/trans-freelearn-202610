# Step six – calculate simple network metrics

At this point, we are ready to run some simple social network metrics using a Python package like networkx. Even though Social Network Analysis (SNA) is beyond the scope of this book, we can still perform a few calculations quite easily without delving too deeply into the mysteries of SNA.

First, we should make sure that we have the networkx package installed. I am using Canopy for my Python editor, so I will use the Package Manager to search for networkx and install it.

Then, once networkx is installed, we can write some quick Python code to read our Pajek file and output a few interesting facts about the structure of my Facebook network:

```
import networkx as net

# read in the file
g = net.read_pajek('fb_pajek.net')

# how many nodes are in the graph?
# print len(g)

# create a degree map: a set of name-value pairs linking nodes
# to the number of edges in my network
deg = net.degree(g)
# sort the degree map and print the top ten nodes with the
# highest degree (highest number of edges in the network)
print sorted(deg.iteritems(), key=lambda(k,v): (-v,k))[0:9]
```

The result for my network looks like the following output. The top ten nodes are listed, along with a count of how many of my other nodes each of these links to:

```
[(u'Bambi', 134), (u'Cinderella', 56), (u'Capn_Crunch', 50),
    (u'Bugs_Bunny', 47), (u'Minnie_Mouse', 47), (u'Cruella_Deville',
    46), (u'Alice_Wonderland', 44), (u'Prince_Charming', 42),
    (u'Daffy_Duck', 42)]
```

This shows that Bambi is connected to 134 of my other friends, but Prince_Charming is only connected to 42 of my other friends.

![](bbox=[0.21, 0.74, 0.25, 0.79])

> If you get any Python errors about missing quotations, double-check your Pajek format file to ensure that all node labels are free of spaces and other special characters. In the cleaning procedure explained in the preceding example, we removed spaces and the quotation character, but your friends may have more exotic characters in their names!