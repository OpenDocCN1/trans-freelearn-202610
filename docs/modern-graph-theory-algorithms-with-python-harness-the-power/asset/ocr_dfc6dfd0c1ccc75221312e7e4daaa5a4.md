Our goal is to spot trends as they happen to pivot business strategy as quickly as possible to meet growing regional demands for supplies and to minimize real estate expenses as rents soar in our metro area. Given that these stores share a geography, we’ll assume that the spatial weight matrix connects all four stores to each other. In a larger chain of stores, creating spatial weights based on demographic characteristics or distance from other stores might hone the analysis.

For now, let’s turn our attention to the code that will help us find insights into our Burkina Faso market data and our COVID-19 store sales data.

# Analyzing our spatiotemporal datasets

Our Burkina Faso market dataset is conveniently broken into quarters, and to understand quarter-by-quarter changes, it makes sense to set our window size as four quarters (a year) with a three-quarter overlap (to focus on monthly changes). As we did in Chapter 2, we’ll calculate the local Moran statistic through the weight matrix and correlation metric. Let’s set up our needed packages and import our Burkina Faso Market dataset plus its weight matrix with Script 7.1:

```
#import packages
import igraph as ig
from igraph import Graph
import numpy as np
import pandas as pd
import os
import matplotlib.pyplot as plt

#import Burkina Faso market millet prices
File="C:/users/njfar/OneDrive/Desktop/BF_Millet.csv"
pwd=os.getcwd()
os.chdir(os.path.dirname(File))
mydata=pd.read_csv(os.path.basename(File),encoding='latin1')

#import weight matrix of Burkina Faso markets
File="C:/users/njfar/OneDrive/Desktop/weights_bk.csv"
pwd=os.getcwd()
os.chdir(os.path.dirname(File))
weights=pd.read_csv(os.path.basename(File),encoding='latin1')
```

Now that we have our data and packages, let’s calculate the Forman-Ricci curvature and PageRank centrality and save the network and overall metrics for each time slice by adding to Script 7.1 with a correlation threshold set to 0.9 (for maximum sensitivity to major changes):

```
#score in yearly sets with 3 quarter overlap
vcurv_t=[]
vcurv_ave=[]
```