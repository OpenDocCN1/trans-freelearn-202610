This dataset is great for practicing Tableau because it contains yearly CO2 emissions, population, and per-capita values across countries; perfect for learning comparisons, trends, and geographic visualization:

+   1. Go to the OWID CO2 dataset repository at https://github.com/owid/co2-data and download the CSV file.

+   2. Save the file locally on your computer. I called it OWID.csv.

+   3. Open Tableau Desktop.

+   4. On the start page, click Connect | To a File | Text File.

+   5. Select the downloaded OWID.csv file:

![](bbox=[0.178, 0.271, 0.23, 0.284])

![](bbox=[0.181, 0.318, 0.29, 0.333])

![](bbox=[0.38, 0.356, 0.663, 0.433])

Need help adding data? Learn more

![](bbox=[0.173, 0.474, 0.842, 0.643])

Figure 1.52: OWID.csv

+   6. Open a new worksheet and drag Year to the Columns shelf, drag Co2 to the Rows shelf, and name the worksheet Emission Trends.

+   7. To include only countries without any aggregations, such as all Organization for Economic Co-operation and Development (OECD) countries, please move ISO Code to the Filter shelf and deselect the Null value. Click on the filter again and select Apply to Worksheets | All Using This Data Source.