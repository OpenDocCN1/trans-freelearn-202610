We’ll choose a window of 5 trading days or roughly a week’s worth of stock data. This allows us to capture trends relevant to day trading, where stocks are traded frequently based on volatility. Our network metrics should work well for volatility-based trading for quick gains, as they capture increasing correlations across the two stocks.

Another aspect that is important to windowing time series data is the choice of overlap. For our example, we’ll choose maximal overlap (4 days’ overlap) to maximize our sensitivity to day-to-day trends. In other applications, less overlap may be desirable to investigate longer time trends. Note that our example uses a path on one of our machines. Your file path will be different from ours.

Let’s import our packages and load our data into Python with Script 6.1 to get started:

```
#import packages
import igraph as ig
from igraph import Graph
import numpy as np
import pandas as pd
import os
import matplotlib as plt

#import stock data
File="C:/users/njfar/OneDrive/Desktop/AAPL_GOOGL_Stock_2004_2020.csv"
pwd=os.getcwd()
os.chdir(os.path.dirname(File))
mydata=pd.read_csv(os.path.basename(File),encoding='latin1')
```

Now that we have our data imported, we can add a loop that windows our time series to 5-day periods that overlap by 4 days across slices. This window strategy yields the best chance to find daily changes in trends. We’ll then create correlations among the four stocks within that time slice, threshold those correlations to limit our analysis to high correlations, create an unweighted network from those thresholds, and analyze Pagerank centrality, degree centrality, betweenness centrality, and Forman-Ricci curvature centrality across the time slices. We’ll also save each unweighted network for future retrieval, as well as the network metrics and their averages over each time slice. Let’s add these pieces to Script 6.1:

```
#script to create time slices, derive networks,
#and compute centrality metrics
stock_networks=[]
bet_t=[]
deg_t=[]
eig_t=[]
vcurv_t=[]
bet_ave=[]
deg_ave=[]
```