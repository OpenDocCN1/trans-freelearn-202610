
Consolidating another HDX dataset will yield information about violent incidents in each area (https://data.humdata.org/dataset/ucdp-data-for-democratic-republic-of-the-congo). While this may not include all information for a region in our time period, we should capture major conflicts or disasters.

Now that we have our data sources, let's explore the data and perform the needed transformations to coax the data into a format that will work as input to our GEE model.

## Data transformation

As we assembled our dataset, we noticed a few caveats in data quality worth mentioning. Very few natural disasters hit the area during our time period of interest, and these were spread across a wide geography. It's unlikely that this factor will impact Ebola spread, but we will keep this variable in our dataset.

The source for violent incidents appears to be missing information, as known attacks on aid workers and Ebola treatment sites in Katwa are not included in the data. This suggests an incomplete data source that may not capture violent incidents at the level needed for a real-world analysis of factors influencing Ebola spread. However, for purposes of demonstrating this method, it captures enough to be potentially interesting in the analysis. In projects such as this, data quality can be questionable, as good sources are hard to find in much of the developing world.

The search for transportation routes was also questionable, yielding some insight into direct routes but not in as much detail as would be ideal. We considered any route between city pairs to be viable if travel would take less than 10 hours by car, bus, or foot travel. Most cities yielded useable data; however, some cities did not show viable routes, yielding isolated vertices in our travel network. No major differences in routing existed across the years (no projects listed to create new roads or bus routes), so our network properties will not change over the years.

The Ebola case source data likely showed some errors, as cumulative cases were sometimes lower over time for certain locations. We erred on the side of retaining positive or zero case totals per year, given that we hope to model our outcome as count data. Cases can be difficult to determine in such outbreaks, as lab confirmation is difficult and often time-consuming when sent to another city for analysis.

Thus, our data collection for this project highlights some of the real-world challenges of building datasets from scratch, particularly in areas where data collection may be incomplete or nonexistent with respect to open source sites. It is often advantageous to examine many potential sources and either merge insights or choose the best source. In our case, we chose the most complete sources to obtain our dataset.

## Python wrangling

From our sources, we’ve assembled a .csv file containing year, city, region, conflict, disaster, and Ebola case data. However, we’ll need to compute **betweenness centrality** and **degree centrality** from our transportation network and add that to our dataset to capture transportation network properties for each city.

Let’s get started by computing the centrality metrics for each city in our transportation dataset so that we can append these values to our .csv file. We’ll compute the metrics using Script 13.1:

```python
#install igraph and pycairo
!pip install igraph
!pip install pycairo

#import igraph, pandas, and numpy
import igraph as ig
from igraph import Graph
import numpy as np
import pandas as pd
import os

#import adjacency matrix
File ="C:/users/njfar/OneDrive/Desktop/DRC_Transport.csv"
pwd = os.getcwd()
os.chdir(os.path.dirname(File))
mydata = pd.read_csv(os.path.basename(File), encoding='latin1', header=None)

#create and plot graph
g_transport=Graph.Adjacency(mydata)
ig.plot(
    g_transport,bbox=(200,200),
    vertex_label = ["Beni","Bunia","Butembo","Kalunguta","Katwa",
    "Mabalako","Mandima"]
)

#calculate betweenness and degree centrality
bet=Graph.betweenness(g_transport)
deg=Graph.degree(g_transport)

#create vector to add to dataset
degree=deg+deg+deg+deg+deg
betweenness=bet+bet+bet+bet+bet
```

```python
    cov_struct=cov_str,
    family=family
)
results=model.fit()
```

This model should fit quickly given the small size of the dataset. Larger models may take a while to run, particularly those with large numbers of clusters and many time points considered. However, this model will run more quickly than other longitudinal models such as generalized linear mixed models, as it does not have a hierarchical structure or likelihood function derivations.

We can now examine our model’s parameters and fit statistics to see what is predictive of Ebola cases within our data by adding to `Script 13.2`:

```python
#examine resulting model
results.summary()
```

This should give a summary similar to *Table 13.1:*

**GEE Regression Results**

| Attribute | Value |
|---|---|
| Dep. Variable: | Ebola_Cases |
| No. Observations: | 35 |
| Model: | GEE |
| No. clusters: | 7 |
| Method: | Generalized Estimating Equations |
| Min. cluster size: | 5 |
| Max. cluster size: | 5 |
| Family: | Poisson |
| Mean cluster size: | 5.0 |
| Dependence structure: | Exchangeable |
| Num. iterations: | 11 |
| Date: | Thu, 21 Dec 2023 |
| Scale: | 1.000 |
| Covariance type: | robust |
| Time: | 10:18:47 |

| Variable | coef | std err | z | P>\|z\| | [0.025 | 0.975] |
|---|---:|---:|---:|---:|---:|---:|
| Intercept | 3.8291 | 0.716 | 5.345 | 0.000 | 2.425 | 5.233 |
| Province[T.North_Kivu] | 1.1145 | 0.699 | 1.593 | 0.111 | -0.256 | 2.485 |
| Violent_Incidents | 0.0312 | 0.022 | 1.415 | 0.157 | -0.012 | 0.074 |
| Disasters | -3.3828 | 0.323 | -10.467 | 0.000 | -4.016 | -2.749 |
| Degree | -0.0098 | 0.093 | -0.105 | 0.917 | -0.192 | 0.173 |
| Betweenness | -0.0806 | 0.975 | -0.083 | 0.934 | -1.991 | 1.830 |

- Bolker, B. M., Brooks, M. E., Clark, C. J., Geange, S. W., Poulsen, J. R., Stevens, M. H. H., & White, J. S. S. (2009). Generalized linear mixed models: a practical guide for ecology and evolution. Trends in ecology & evolution, 24(3), 127-135.
- Breslow, N. E. (1996). Generalized linear models: checking assumptions and strengthening conclusions. Statistica applicata, 8(1), 23-41.
- Gatherer, D. (2014). The 2014 Ebola virus disease outbreak in West Africa. Journal of General Virology, 95(8), 1619-1624.
- Groseth, A., Feldmann, H., & Strong, J. E. (2007). The ecology of Ebola virus. Trends in microbiology, 15(9), 408-416.
- Guetiya Wadoum, R. E., Sevalie, S., Minutolo, A., Clarke, A., Russo, G., Colizzi, V., ... & Montesano, C. (2021). The 2018–2020 ebola outbreak in the Democratic Republic of Congo: A better response had been achieved through inter-state coordination in Africa. Risk management and healthcare policy, 4923-4930.
- Hanley, J. A., Negassa, A., Edwardes, M. D. D., & Forrester, J. E. (2003). Statistical analysis of correlated data using generalized estimating equations: an orientation. American journal of epidemiology, 157(4), 364-375.
- Holmes, E. C., Dudas, G., Rambaut, A., & Andersen, K. G. (2016). The evolution of Ebola virus: Insights from the 2013–2016 epidemic. Nature, 538(7624), 193-200.
- Jacob, S. T., Crozier, I., Fischer, W. A., Hewlett, A., Kraft, C. S., Vega, M. A. D. L., ... & Kuhn, J. H. (2020). Ebola virus disease. Nature reviews Disease primers, 6(1), 13.
- Liang, K. Y., & Zeger, S. L. (1986). Longitudinal data analysis using generalized linear models. Biometrika, 73(1), 13-22.
- McKinney, W., Perktold, J., & Seabold, S. (2011). Time Series Analysis in Python with statsmodels. Jarrodmillman Com, 96-102.
- Nelder, J. A., & Wedderburn, R. W. (1972). Generalized linear models. Journal of the Royal Statistical Society Series A: Statistics in Society, 135(3), 370-384.
- Vossler, H., Akilimali, P., Pan, Y., KhudaBukhsh, W. R., Kenah, E., & Rempała, G. A. (2022). Analysis of individual-level data from 2018–2020 Ebola outbreak in Democratic Republic of the Congo. Scientific Reports, 12(1), 5534.
- WHO Ebola Response Team. (2014). Ebola virus disease in West Africa—the first 9 months of the epidemic and forward projections. New England Journal of Medicine, 371(16), 1481-1495.
