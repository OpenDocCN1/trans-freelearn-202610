
## Summary

In this chapter, we’ve introduced some common uses of time series data and time series analytics, including stock market data. We explored several vertex- and edge-based centrality metrics that are common in network analytics. Then, we applied network metrics to a time series problem on NASDAQ stock data from 2004 to 2020 to investigate how network metrics and time series thresholding impact the ability to extract useful information from time series data, such as our stock data. Finally, we investigated extending networks to simplicial complexes and constructed a network by building two simplicial complexes using the Vietoris–Rips method and various threshold values. In Chapter 7, we’ll look at sales and goods pricing across both time and geography to see how network science can solve problems in spatiotemporal data.

## References

De Floriani, L., & Hui, A. (2005, July). Data Structures for Simplicial Complexes: An Analysis And A Comparison. In Symposium on Geometry Processing (pp. 119-128).

Durbach, I., Katshunga, D., & Parker, H. (2013). Community structure and centrality effects in the South African company network. South African Journal of Business Management, 44(2), 35-43.

Estrada, E., & Ross, G. J. (2018). Centralities in simplicial complexes. Applications to protein interaction networks. Journal of theoretical biology, 438, 46-60.

Johansen, A., & Sornette, D. (1998). Stock market crashes are outliers. The European Physical Journal B-Condensed Matter and Complex Systems, 1, 141-143.

Rodrigues, F. A. (2019). Network centrality: an introduction. A mathematical modeling approach from nonlinear dynamics to complex systems, 177-196.

Salnikov, V., Cassese, D., & Lambiotte, R. (2018). Simplicial complexes and complex systems. European Journal of Physics, 40(1), 014001.

Samal, A., Pharasi, H. K., Ramaia, S. J., Kannan, H., Saucan, E., Jost, J., & Chakraborti, A. (2021). Network geometry and market instability. Royal Society open science, 8(2), 201734.

Valente, T. W., Coronges, K., Lakon, C., & Costenbader, E. (2008). How correlated are network centrality measures? Connections (Toronto, Ont.), 28(1), 16.

Xiong, J., & Xiao, W. (2021). Identification of key nodes in abnormal fund trading network based on improved pagerank algorithm. In Journal of Physics: Conference Series (Vol. 1774, No. 1, p. 012001). IOP Publishing.

## An introduction to spatiotemporal data

In Chapter 2, we discussed some aspects of spatial and temporal data that make such data difficult to wrangle, analyze, and scale with statistical and machine learning methods. Recall that both types of data are not independent, with dependencies crossing either space or time in the dataset. Alternative approaches to prediction are needed, and representations of spatial and temporal data as a network allow analysts to leverage the tools of network science to find change points, mine for patterns, and even predict future system behavior in the case of temporal data. In the previous chapters, we’ve learned how to analyze both spatial and temporal data with network science tools such as centrality to understand important trends.

Many analytic tasks in the real world involve spatiotemporal data—including the analysis of world stock markets influenced by local policies, data mining of store data across locations for trends in customer behavior and revenue, discerning disease sources and population vulnerabilities during an epidemic, tracking human behavior (such as crime, eating habits, substance use, birth rates, or terrorism) across countries or regions of a specific country, and demonstrating the effects of climate change across vulnerable countries.

Let’s get acquainted with our two datasets and problems relevant to many applications of spatiotemporal data.

## The Burkina Faso market dataset

In Chapter 2, we introduced the Burkina Faso market dataset, which recorded market millet prices across the country from Quarter 2 of 2015 to Quarter 2 of 2022. During this period, world grain prices fluctuated due to COVID-19 supply chain issues and the Ukraine conflict limiting food distribution and aid. Food security is a major issue in many regions of the world, and understanding what might cause prices to soar out of reach for the average person or family is critical in meeting the world’s food needs and avoiding famine.

Some regions of a country may be more vulnerable to food shortages or soaring costs of goods than others, and the spatial features in our dataset capture these for the Burkina Faso markets. We’d imagine a rural market (such as the one shown in Figure 7.1) would face greater pricing and availability instability than a market in the heart of a city such as Ouagadougou. In addition, some events within a period may cause more pricing volatility than other events in that same period. Capturing the time series trends is also important to understand the causes and potential effects of events on grain prices in the region. Thus, capturing the data as spatiotemporal data allows us to analyze both trends across locations and across time.

![](img/71ff737206f7d80bb625b3c9aca9dc7f_143_0.png)

Figure 7.1 – An illustration of a rural market in Burkina Faso

To capture spatial trends, we’ll return to the weight matrix introduced in Chapter 2, which connected markets located in provinces that share a border. This roughly captures the geography of Burkina Faso, allowing regions that share many borders the potential to influence each other’s pricing and demonstrating the potential supply chain issues related to remote regions of the country.

Just as pricing can change over time and geography, so can sales volumes. Let’s turn to our next example and explore how sales can change across stores due to COVID-19 changes in work policies and the movements of populations across a city.

## Store sales data

Let’s consider a local chain of printer supply stores in a medium-sized city. As populations change near store locations, it may be necessary to pivot business strategy: closing stores that are struggling to minimize real estate costs, preferentially stocking certain stores that are doing well, and opening new locations in high-volume sales areas. Perhaps work-from-home initiatives during COVID-19 have resulted in more people working from home in certain areas of the city and more people moving to certain areas as they have the freedom to live where they would like to live rather than live close to their offices. Perhaps return-to-office initiatives also impact demand for home office supplies. All these factors can impact the printer supply stores and change quickly.

To explore this further, we’ve created a simulated dataset with sales from four stores serving a single metropolitan area that mimic the population movement trends heralded by COVID-19 policies and subsequent waffling on return-to-office versus remote work policies. Let’s say Store 1 (Figure 7.2) serves a population that mainly works in nursing, grocery store management, and other jobs that do not stay remote for very long. This area also has temporary school closures. We’d expect to see a bump in sales initially during COVID-19 lockdowns but a quick return-to-normal pattern of sales:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_144_0.png)

Figure 7.2 – An illustration of a printer supply store – Store 1

However, Store 2 is located near the downtown area and serves a population of mostly mid-level technical workers, who largely don’t fall under return-to-work policies once they are fully remote. Thus, sales spike during the initial lockdown and stabilize to the new normal for that area of town.

Store 3 serves a very desirable suburb near the downtown area mainly comprised of high-earning professionals who either worked from home or went into the office regularly before COVID-19. The commute is typically long, but the school system and safety of the neighborhood are excellent. During COVID-19, this suburb experienced a real estate boom as families elsewhere in the state decided to settle in that area now that they could work from home. Thus, by 2023, the area will have a large remote-worker population in need of printer supplies. Store 3 may need a larger space by 2023, as shown in Figure 7.3:
