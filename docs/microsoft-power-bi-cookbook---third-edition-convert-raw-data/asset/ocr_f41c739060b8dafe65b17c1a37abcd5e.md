# 2

# Accessing, Retrieving, and Transforming Data

Power BI Desktop and dataflows in the Power BI service contain a rich set of connectors and transformation capabilities that support the integration and enhancement of data from many different sources. These features are all driven by a powerful functional language and query engine, M, which leverages source system resources when possible and can greatly extend the scope and robustness of the data retrieval process beyond what’s possible via the standard query editor interface alone. As with almost all BI projects, the design and development of the data access and retrieval process have significant implications for the analytical value, scalability, and sustainability of the overall Power BI solution.

In this chapter, we dive into both Power BI Desktop’s Get Data experience and the equivalent process with dataflows in the Power BI service in establishing and managing data source connections and queries. Although these examples are implemented within Power BI Desktop, Microsoft has made Power Query (M) as a common option for several other data tools and services, such as Excel, Power Platform, and Fabric, thus enhancing the value of Power Query (M) skills and experience. Examples are provided of using the Power Query Editor interface and the M language directly, to construct and refine queries to meet common data transformation and cleansing needs. In practice and as per the examples, a combination of both tools is recommended to aid the query development process.

A full explanation of the M language and its implementation in Power BI is outside the scope of this book, but additional resources and documentation are included in the sections titled There’s more... and See also.

The recipes included in this chapter are as follows:

+   - Importing data
+   - Using DirectQuery
+   - Creating dataflows
+   - Applying filters
+   - Transforming and cleansing source data