# 6
Understanding Delta Lake

In the previous chapter, we created the bronze layer of the lakehouse. The bronze layer stores raw data in the native form as collected from the data sources. The problem is that raw data is not in a shape that can be readily consumed for analytical operations.

As a data engineer, it is your responsibility to convert raw data into a shape and form that becomes ready for use analytical workloads. In this chapter, we will further advance our learning to cleanse raw data. The process of cleansing data involves applying the logic that cleans and standardizes data followed by writing it to the silver layer of the lakehouse.

But that is not all – the silver layer should store data in an open format that supports **ACID (atomicity, consistency, isolation, and durability)** transactions. This is done by using the **Delta Lake** engine. Before we start building the silver layer, we need to completely understand some critical features of Delta Lake and how they relate to the lakehouse architecture.