# Connecting to data sources

One of the beauties of Tableau is the ease with which you can connect to many different data sources in various ways. As mentioned earlier in this book, there are many connectors defined in Tableau for interfacing with a variety of data sources. Furthermore, this flexibility extends beyond simply connecting to single tables or files.

> **Note**
> 
> Although Tableau makes it easy to connect to various data sources, it should be stressed that Tableau is not an **Extract, Transform, and Load (ETL)** tool. If complex joins and complex data blending are required to generate useful results, it may be advisable to perform ETL work outside of Tableau, for example, in Tableau Prep Builder (see _Chapter 3, Using Tableau Prep Builder_, for more information on this service). ETL work will ideally lead to better data modeling and thus easier authoring and quicker performance in Tableau.

The four ways in which Tableau connects to data are as follows:

+   Tableau may connect to a single table. This is ideal as it allows the most functionality and easiest troubleshooting while enabling Tableau to send the simplest queries and thus perform optimally. However, it is not always possible to connect to a single table and, although ideal, it is not reasonable to have such a strict limitation. The relationship between data sources and reporting tools is constantly changing. A reporting tool that is inflexible in the way it can connect to data will likely not be successful, no matter how elegant and beautiful the end results.

+   The second option is relationships, a newer, more flexible way to combine two or more datasets. The level of detail will be defined per worksheet rather than in the **Data Source** tab, which makes this feature very powerful.

+   The third way in which Tableau may connect to data is via joining. One table may not supply all the necessary data, but by joining two or more tables, all the needed data may be accessible. As the joins become more and more complex, performance may be impacted, and troubleshooting may become difficult. Fortunately, Tableau can assume referential integrity and thus work quite efficiently with even complex joins. More information can be found here: https://help.tableau.com/current/pro/desktop/en-us/joins_xbd_perf.htm.

+   Finally, Tableau may utilize data blending. Data blending often performs admirably, provided no more than one of the blended data sources is large and dimensions that are used for blending have relatively few members.

Relationships should be chosen over joining and joining should be chosen instead of blending whenever possible. When blending multiple, large data sources, performance can be seriously impacted. The problem is further compounded when blending on high-cardinality dimensions. Also, data blending limits some functionality, such as the ability to use dimensions, row-level calculations, or LOD expressions, from a secondary data source.