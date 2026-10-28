
## HUKUM NUN MATI DAN TANWIN

| No | Nama Hukum | Huruf | Cara Membaca | Contoh |
|---|---|---|---|---|
| 1 | Izhar Halqi (إظهار حلقى) | ء ه ع ح غ خ | Jelas tanpa dengung | مِنْ أَنْعَمْتَ عَلَيْهِمْ |
| 2 | Idgham Bigunnah (إدغام بغنة) | ي ن م و | Melebur dengan dengung | مَنْ يَعْمَلْ |
| 3 | Idgham Bilagunnah (إدغام بلاغنة) | ل ر | Melebur tanpa dengung | مِنْ لَدُنْ |
| 4 | Ikhfa Haqiqi (إخفاء حقيقى) | ت ث ج د ذ ز س ش ص ض ط ظ ف ق ك | Samar dengan dengung | مِنْ قَبْلُ |
| 5 | Iqlab (إقلاب) | ب | Berubah menjadi mim dengan dengung | مِنْ بَعْدِ |
| 6 | Gunnah (غنة) | ن م | Dengung pada nun dan mim bertasydid | إِنَّ |

![](img/5d2bce74fd6fceebed14fd1488bf7088_219_0.png)

Figure 4.45: Turning on the Enhanced compute engine on a Power BI dataflow

- 3. Refresh the dataflow after turning the Enhanced compute engine settings on

Now that we meet the requirements, we can connect to a dataflow from Power BI Desktop in DirectQuery mode.

## Power BI Datamarts

The term Datamart is related to data warehousing where users have access to relevant and business-centric data, while a data warehouse is usually enterprise-wide, covering a broad range of historical and current data.

Microsoft introduced Datamart capability for Power BI Premium licenses in May 2022, enabling organizations to leverage governed and secure self-service data transformation without seeing the IT team as a bottleneck in the data migration processes. Organizations traditionally have an IT team taking care of the data migration and transformation from the source systems into central storage.

This process is usually complex, cumbersome, and time-consuming, leading the business teams to see the IT team as a bottleneck. In response, business users started building their own data stores using local SQL Server instances, Excel files, SharePoint site, or Microsoft Access files to cover

![](img/5d2bce74fd6fceebed14fd1488bf7088_221_0.png)

Figure 4.47: Connect Live to a Power BI dataset linked to a datamart

## SQL Server

SQL Server is one of the most common data sources for Power BI. When connecting to a SQL Server database, we have two options: importing the data into the data model or connecting to the database in DirectQuery mode. This chapter discusses the connection modes in later sections. Follow these steps to get data from a SQL Server data source:

- 1. Click the SQL Server button from the Home tab.
- 2. Enter the Server name.
- 3. We may want to enter the Database name (optional) depending on our scenario.
- 4. We can either select Import or DirectQuery.
- 5. Again, depending on our scenario, we may want to enter Transact-SQL (T-SQL) statements. To do so, click to expand the Advanced options.
- 6. Leave the Include relationship columns option ticked (untick this option to stop Power BI from detecting related tables when selecting

a table from the Navigator page and clicking the Select Related Tables option).

- 7. Leave the Navigate using full hierarchy option unticked (tick this option if you would like to see the navigation based on database schemas).

- 8. If you have High Availability (HA) settings on the SQL Server instance, then tick the Enable SQL Server Failover support item; otherwise, leave it unticked.

- 9. Click OK.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_222_0.png)

Figure 4.48: Getting data from SQL Server

- 10. Tick a desired table from the list. This option is only available if we do not enter T-SQL statements.

- 11. Click the **Select Related Tables** button.

- 12. Depending on what you need to do next, you can either click the **Load** button to load the data into the data model or click the **Transform Data** button, which navigates you to the **Power Query Editor**.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_223_0.png)

Figure 4.49: Selecting a table and related tables from the Navigator window

Note
As shown in step 5 in Figure 4.48, typing T-SQL statements disables query folding in Power Query, which potentially causes some performance degradation during the data refresh. However, there is a technique with which we can enforce query folding when using T-SQL

build a single source of truth within an enterprise-grade semantic layer in Power BI. We cover composite models with live connection in *Chapter 14, DirectQuery Connections to Power BI Datasets and Analysis Services in Composite Models.*

## SSAS multidimensional/tabular

To connect to an instance of SSAS, follow these steps:

- 1. Click the **Get data** dropdown.
- 2. Click **Analysis Services**.
- 3. Enter the **Server** name.
- 4. Enter the **Database** name (optional).
- 5. Select the connection mode; if you select **Import**, you could write **Multi-dimensional Expressions (MDX)** or **DAX** expressions to get a result set.
- 6. Click **OK**.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_225_0.png)

Figure 4.50: Connecting to SSAS

- 7. Select the model.
- 8. Click **OK**.

The preceding steps are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_226_0.png)

Figure 4.51: Connecting to the model

## AAS

Connecting to an instance of AAS is similar to connecting to an instance of SSAS. The only difference is that this time, we use its dedicated connector. We can find the **Azure Analysis Services database** connector under the **Azure** folder from the **Get Data** window, as shown in the following image, so we will not go through the steps again:

![](img/5d2bce74fd6fceebed14fd1488bf7088_227_0.png)

Figure 4.52: Connecting to an AAS tabular model

An extra option available on the **Azure portal** for AAS databases is downloading the PBIX file directly from the Azure portal. The downloaded PBIX file contains the connection to the desired model in AAS. The following steps explain how to do this:

- 1. After logging in to the Azure portal, navigate to your instance of AAS, then click **Overview**.
- 2. Find the model you want to connect to and click the ellipsis button on the right.
- 3. Click **Open in Power BI Desktop**.

The following image shows the preceding steps:

- 3. Enter your PWA Uniform Resource Locator (URL). It must look like this: https://Your_Tenant.sharepoint.com/sites/pwa/_api/.
- 4. Click OK.
- 5. Click Organizational account.
- 6. Click the Sign in button, then pass your credentials.
- 7. Click Connect.

The following image shows the preceding steps:

![](img/5d2bce74fd6fceebed14fd1488bf7088_229_0.png)

Figure 4.54: Connecting to OData feed

- 8. Select the desired tables; in our sample, we selected the Projects table.
- 9. Click either Load or Transform Data; we selected the Transform Data option.

The preceding steps are highlighted in the following image:

## Figure 4.55: Selecting tables from the Navigator page

As shown in the following image, the Projects table has 131 columns:

## Figure 4.56: The Projects table from the PWA data source in Power Query

We usually do not require the loading of all columns into the data model. Therefore, having a good understanding of the underlying database structure is essential. If we do not know much about the underlying data

source, it is wise to involve **subject-matter experts (SMEs)**. They can help identify the more relevant columns in the business.

## Dataverse

Microsoft Dataverse, formerly named **Common Data Services (CDS)**, is a data service and application platform supporting multiple connected applications and processes under the Microsoft Power Platform umbrella. Dataverse provides a cloud-based central repository for Power Platform solutions such as **PowerApps, Power Automate, Power Virtual Agent, and Power Pages**. We can also integrate **Dynamic 365 (D365)** solutions such as **Business Central (BC)** with Dataverse.

Follow these steps to connect to Dataverse:

- 1. Click the **Dataverse** button from **Home**
- 2. Click **Sign in**
- 3. Pass your credentials and click **Connect**
- 4. Expand the desired environment
- 5. Select the desired table
- 6. Depending on what you want to do next, click **Load** or **Transform Data**
- 7. Select the connection type (**Import** or **DirectQuery**)
- 8. Click **OK**

The following image shows the preceding steps:
