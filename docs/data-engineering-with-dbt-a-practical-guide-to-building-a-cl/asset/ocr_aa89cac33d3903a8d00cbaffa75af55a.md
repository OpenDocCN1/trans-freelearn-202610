Looking at the results, you can see that with this kind of data structure and query, you can calculate fields over the full path traversed in the hierarchy.

You can refer to Chapter 1 for more info on CTEs.

# Forecasts and actuals

In companies, it is very common to have budgeting and forecasting processes that try to predict some figures in the future and then want to analyze them by matching the different forecasts done over time with the actuals once they become known.

One of the peculiarities of the model in this case is that between the source data model and the reporting data model it is common to create two very different data models even if the entities involved do not change.

Generally, we have one actual value for each period and unit, and many forecasts that have been done over time for the same period and unit. We therefore have a one-to-many relationship between Actuals and Forecasts.

When we want to analyze Actuals and Forecasts together for reporting, we want to be able to associate the actual figure, once it is available, with each forecast. We can therefore consider them as two subclasses or specialized versions of the Figures entity, which is the common generalization.

In the following figure, we use the UML notation to visualize the source data model and the reporting data model, which uses the generalization-specialization notation. It is important to note that generalization is not a relationship in the E-R sense but indicates derivation:

![](bbox=[0.13, 0.55, 0.88, 0.7])

Figure 3.19: Data models for Actuals and Forecasts

The specialized entities are made up of the attributes inherited from the general entity plus the attributes specific to the specialized entity.

While it is possible to implement the reporting model in multiple ways, the most common is to have all specialized entities in a single table with one column for each different attribute of all the specialized entities. Compatible attributes, such as Actuals_Value and Forecast_Value, in our example, are stored in the same field, to make their use/comparison simpler.