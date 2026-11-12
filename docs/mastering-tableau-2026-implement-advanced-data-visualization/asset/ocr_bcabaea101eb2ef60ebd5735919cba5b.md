![](bbox=[0.09, 0.08, 0.86, 0.55])

Figure 4.43: Forecast dashboard

We are obviously already many years ahead; can you use this dashboard to figure out the actual average growth rate for China and India from 2012 to 2020 if I tell you that the population in 2020 was 1,439,323,776 in China and 1,380,004,385 in India? Share your results on Tableau Public with the #MasteringTableau tag!

One key to this exercise is data scaffolding. Data scaffolding produces data that does not exist in the data source. The World Indicators dataset only includes dates from 2000 to 2012, and obviously, the Records dataset does not contain any dates. By using the Start Date and End Date parameters coupled with the calculated Date field, we were able to produce any set of dates desired. We had to blend the data, rather than using a join or a union, to keep the original data source intact and create all additional data outside of World Indicators itself.

The actual data scaffolding occurred upon selecting Show Missing Values from the Date field dropdown after it was placed on the Rows shelf. This allowed every year between Start Date and End Date to display, even when there were no matching years in the underlying data. Chapter 6, Utilizing OData, Data Densification, Big Data, and Google BigQuery, will explore something remarkably similar to data scaffolding-data densification-in more detail.