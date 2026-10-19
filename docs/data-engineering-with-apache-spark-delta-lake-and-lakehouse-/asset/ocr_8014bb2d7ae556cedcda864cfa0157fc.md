Generally, in any data lake operation, we ingest data to storage, process it as per requirements, and finally publish the aggregated data to a warehouse so that it is available for analytics.

Lately, a new architecture is being adopted at a fast rate that combines the power of a data lake and a data warehouse in one, as illustrated in the following diagram:

![](bbox=[0.18, 0.26, 0.82, 0.35])

# Data Lake and Data Warehouse are Decoupled

![](bbox=[0.18, 0.4, 0.82, 0.55])

# Data Lake and Data Warehouse are Merged

Figure 2.21 – Rise of the Lakehouse architecture

In this section, we learned about the Kappa and Lambda architecture, and the rise of the Lakehouse architecture.

# Lakehouse architecture

Before we discuss the Lakehouse architecture, it is important to understand what I refer to as the power struggle between traditional and modern data processing systems.

# The give-and-take struggle

There was a card game that I used to play when I was younger. In this game, to advance to the next level, you were required to sacrifice your best card.

Through many years in the data processing world, I have seen the same give-and-take struggle between the traditional method of data processing (database/data warehouses) versus the modern one (data lakes).