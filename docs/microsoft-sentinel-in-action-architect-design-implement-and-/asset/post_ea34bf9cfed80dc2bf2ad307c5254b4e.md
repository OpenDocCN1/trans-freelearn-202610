
Now that the Microsoft Graph Security API extension has been set up on the MineMeld server, we can configure the Microsoft Sentinel connector.

## Configuring the Microsoft Sentinel connector

The following are the steps to configure the API extension:

- 1. Log in to the MineMeld web portal. Go to CONFIG, then select the prototypes icon at the bottom of the page with three horizontal lines (that is, the hamburger menu), as shown in the following screenshot:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_124_0.png)

Figure 4.14 – MineMeld config page

2. As you will see, there are many options to choose from. Review the list for additional TI feeds you may want to implement, then search for Microsoft and review the results. Select the prototype named microsoft_graph_secapi.output:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_125_0.png)

Figure 4.15 – Microsoft prototypes

4. On the next page, provide a name for the node, then select inboundaggregator for the INPUTS field:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_127_0.png)

Figure 4.17 – Configuring a new node

5. Confirm the new node is active by viewing the list of nodes, as shown in the following screenshot. Select the COMMIT button at the top of the page. This will save the changes and restart the process to enable the new functionality:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_128_0.png)

Figure 4.18 – Confirming the new node is added

6. Now go to the NODES tab and select the new API extension, then enter the relevant details from the previously registered app in Azure AD (see Figure 4.6):

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_129_0.png)

Figure 4.19 – Configuring the API extension for Microsoft Sentinel

7. For the **CLIENT SECRET** field, you need to generate a new secret in the Azure AD app. Go to the Azure AD app created earlier and select **Certificates & secrets**, and then select **+ New client secret** and follow the steps that will appear to create a new secret (ensure you copy the details because you only get one chance; otherwise, you will need to generate a new client secret again):

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_130_0.png)

Figure 4.20 – Creating a new client secret

8. With this new secret added to the settings, you should see the following completed screen:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_131_0.png)

Figure 4.21 – Completed configuration

This concludes the configuration requirements for the MineMeld service. TI feeds should now be sent to your Microsoft Sentinel instance.

## Confirming the data is being ingested for use by Microsoft Sentinel

To confirm the data is being sent to Microsoft Sentinel, follow these steps:

- 1. Go to the Azure portal, then Microsoft Sentinel.
- 2. Select Logs.
- 3. Type the following command into the command window and select Run:

```
ThreatIntelligenceIndicator
| take 100
```

4. You should see the following screen with the results:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_132_0.png)

Figure 4.22 – Checking for TI feed activity

5. The following screenshot shows an example of the details for one of the events:

Completed. Showing partial results from the last 24 hours.

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_133_0.png)

Figure 4.23 – TI data feed details

You have now configured and connected the MineMeld server to your Microsoft Sentinel workspace and can see the TI data feeds appearing in the logs. You can now use this information to help to create new analytics and hunting queries, notebooks, and workbooks in Microsoft Sentinel.

We recommend regular reviews to ensure this information is both relevant and updated frequently. New TI feeds become available regularly, and you don't want to miss out on that useful information if it can help you to find new and advanced threats.

## Summary

In this chapter, we explored the concept of TI, the new terminology and solution options, and the concept of creating and sharing TI feeds as a community effort. There are several options available for adding TI feeds to Microsoft Sentinel, and we know Microsoft is working to develop this even further. TI feeds will assist with the analysis and detection of unwanted behavior and potentially malicious activities. With many options to choose from, selecting the right feeds for your organization is an important part of configuring Microsoft Sentinel.

The next chapter introduces the Kusto Query Language (KQL), which is a powerful means to search all data collected for Microsoft Sentinel, including the TI data we just added.

## Questions

- 1. Name three examples of threat indicator types.
- 2. What is the full name of the ATT&CK framework?
- 3. Which Microsoft Sentinel components can utilize TI feeds?
- 4. Who developed the STIX language and the TAXII protocol?

## Further reading

The following resources can be used to further explore some of the topics covered in this chapter:

- - Malware Information Sharing Project: https://www.misp-project.org/
- - Mitre ATT&CK framework: https://attack.mitre.org/
- - Microsoft Security Graph API: https://github.com/microsoftgraph/security-api-solutions/tree/master/QuickStarts
- - STIX standard: https://stixproject.github.io/
- - TAXII protocol: https://www.mitre.org/sites/default/files/publications/taxii.pdf
- - Build a MineMeld server: https://live.paloaltonetworks.com/t5/general-articles/manually-install-minemeld-on-ubuntu-16-04/ta-p/253336
- - The Microsoft Graph Security API extension in MineMeld: https://github.com/PaloAltoNetworks/minemeld-msgraph-secapi

## 5 Using the Kusto Query Language (KQL)

The Kusto Query Language (KQL) is a plain-text, read-only language that is used to query data stored in Azure Log Analytics workspaces. Much like SQL, it utilizes a hierarchy of entities that starts with databases, then tables, and finally columns. In this chapter, we will only concern ourselves with the table and column levels.

In this chapter, you will learn about a few of the many KQL commands that you can use to query your logs.

In this chapter, you will learn the following:

- - How to test your KQL queries
- - How to query a table
- - How to limit how many rows are returned
- - How to limit how many columns are returned
- - How to perform a query across multiple tables
- - How to graphically view the results

The chapter covers the following main topics:

- - Running KQL queries
- - Introduction to KQL commands
- - Query statements
- - Scalar functions

## Running KQL queries

For this chapter, we will be using the sample data available in the **Azure Data Explorer (ADX)**. This is a very useful tool for trying simple KQL commands. Feel free to use it to try the various commands in this chapter. All the information used in the queries comes from the sample data provided at https://dataexplorer.azure.com/clusters/help/databases/Samples.

If prompted, use the login credentials you would use to log in to the Azure portal. When you log in for the first time, you will see the following screen. Note that your login name may show up on the right-hand side of the header:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_136_0.png)

To run the samples for this chapter, you will need to expand the Samples logs on the left-hand side of the screen and then select StormEvents. You can expand StormEvents to see a listing of fields if you want to. If you do so, your screen should look like the following:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_137_0.png)

Figure 5.2 – StormEvents

To run a query, either type or paste the command into the query window at the top of the page, just to the right of the line numbered 1 in the preceding screenshot. Once the query has been entered, click on the Run button to run the query. The output will be shown in the results window at the bottom of the page.

In the following screenshot, a simple query was entered and run. The query was entered in the query window, and you can see that the results are shown below it:

![](img/387db4214c5424f3a9b9dfa5ccc56a9d_138_0.png)

Figure 5.3 – Executed query

There is a lot more you can do with the ADX, so feel free to play around with it. Now that you understand how we are going to run the queries and view the results, let's look at some of the various commands KQL has to offer.

## Introduction to KQL commands

Unlike SQL, the query starts with the data source, which can be either a table or an operator that produces a table, followed by commands that transform the data into what is needed. Each command's output can be passed into the next command by using the pipe ( | ) delimiter.

What does this mean? If you are familiar with SQL, you would write a statement such as Select * from table to get the values from the table. The same query in KQL would just be table, where table refers to the name of the log. It is implied that you want all the columns and rows. Later, we will discuss how to minimize what information is returned.

We will only be scratching the surface of what KQL can do here, but it will be enough to get you started writing your own queries so that you can develop queries for Microsoft Sentinel.
