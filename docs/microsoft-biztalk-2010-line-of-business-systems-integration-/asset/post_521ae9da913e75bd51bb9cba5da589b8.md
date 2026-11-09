
![](img/23d1402649b6528b085cd6b900a2c9c3_354_0.png)

Note: The BizTalk Host instance user for the Receive Location that will be connecting to SharePoint requires "Contributor" access to the document or form library. For additional information regarding SharePoint permissions, please refer to the following link: http://technet.microsoft.com/en-us/library/cc721640.aspx

10. Our Send Port will leverage the FILE Adapter to deliver the approved flat file vacation request to a file drop that our ERP system is capable of consuming from. Since the ERP system is expecting a flat file, we need to ensure that our custom Send Pipeline is being used. The pipeline that we want to use is Send Vacation Approval.

![](img/23d1402649b6528b085cd6b900a2c9c3_355_0.png)

11. The configuration for the processVacationRequests orchestration is very straightforward:

- We have one Receive Port for receiving the InfoPath Forms from SharePoint
- A Send Port that we will use to send the Approved Vacation Requests to the ERP system
- The Host we will use is BizTalkServerApplication. Remember to restart this Host Instance, before trying to execute the sample application.

![](img/23d1402649b6528b085cd6b900a2c9c3_355_1.png)

## Testing the application

At this point, we are ready to validate our solution. To test our solution, we need to perform the following:

1. Navigate to your SharePoint document library and select **Add Document**.
2. Populate the InfoPath form and click the **Submit** button.

![](img/23d1402649b6528b085cd6b900a2c9c3_356_0.png)

3. You should now have a document stored in your form library that has an Approval Status of **Pending.**

![](img/23d1402649b6528b085cd6b900a2c9c3_356_1.png)

4. Open this existing form, set the **Approval Status** to **Approved**, and then click the **Submit** button.

5. We will momentarily see that the Approval Status of the InfoPath has changed to Approved.

![](img/23d1402649b6528b085cd6b900a2c9c3_357_0.png)

6. Since the Approval Status has changed to Approved, BizTalk will now consume this message and write it to our folder location.

![](img/23d1402649b6528b085cd6b900a2c9c3_357_1.png)

7. If we open up the file, we will determine that it matches the format that the ERP system is expecting.

![](img/23d1402649b6528b085cd6b900a2c9c3_357_2.png)

## Sending documents to SharePoint

In this scenario, we have a Work Order Management system that employees use to manage their job, or tasks, for an Oil and Gas company. A legal requirement for this organization is they must record any safety incidents that occur in the field for their employees. Employees will use this Work Order Management system to update the statuses of their jobs, but will also record any Field incidents. The problem with capturing this information in the Work Order Management system is that it has been designed for the field and is not conducive for office employees that may be required to track these incidents to ensure regulatory compliance.

The solution to this problem is to allow the field staff to input the incident data in the Work Order Management system, but to feed this info into SharePoint where it can be tracked by office employees. The Work Order Management system will output an XML file that we will load into a SharePoint Form library.

Since we do not want office employees reviewing XML documents, we will load this information into an InfoPath form, which will allow for a nice user interface for our office employees.

Another benefit of having this information in SharePoint is that we can apply an Information Policy which will set the stale date of the document and automatically purge it if the date of the document exceeds the document's expiration date.

## Creating the SharePoint Forms Library

In the Receiving Documents walkthrough, we had the InfoPath client create a Forms Library for us. This time around we will manually create a Content Type, a Forms Library, and the SharePoint Columns. A Content Type is a template that we can use to base content on. It allows us to set a default document type within the SharePoint library so if we want to create a new document from the SharePoint ribbon, by default it will be the type that we based the Content Type upon.

1. Before we create a Content Type, we need to create an InfoPath form that we can base the Content Type upon. We will once again start with an XSD schema. In this particular case, the name of the schema that we will be using is **WorkOrderManagementIncident_InfoPath.xsd**, which can be found in the Chapter7-SendingDocuments solution.

2. We then want to design an InfoPath form based upon the **WorkOrderManagementIncident_InfoPath** schema much like we did in the Receiving Documents from SharePoint section in this chapter. The difference being that we will not publish this form to SharePoint, nor will we submit the form to SharePoint as we are basically using InfoPath as a viewer for these documents and will not be modifying them.

![](img/23d1402649b6528b085cd6b900a2c9c3_359_0.png)

3. We now need to create a Content Type and associate this InfoPath form with it. To do this, click on the Site Actions drop down and then click on Site Settings.

![](img/23d1402649b6528b085cd6b900a2c9c3_360_0.png)

4. Now, we need to click on the Site Content Types from the Galleries grouping.

![](img/23d1402649b6528b085cd6b900a2c9c3_360_1.png)

5. Click on the Create button.

![](img/23d1402649b6528b085cd6b900a2c9c3_360_2.png)

6. Provide a **Name**, the **Parent Content Type**, and then select the group that this site content should belong to. In this case, we should use **Custom Content Types.**

![](img/23d1402649b6528b085cd6b900a2c9c3_361_0.png)

7. As we saw in the Receiving Documents sample from this chapter, SharePoint columns become a valuable tool for associating metadata to documents inside SharePoint. By populating SharePoint columns, we can now search, sort, and create views based upon the information contained within the columns.

- We will now associate columns to our particular Content Type so that when we add our Content Type to our document library, these columns will be available for us to populate using the Windows SharePoint Services Adapter. Click on the **Add from new site column** link.

![](img/23d1402649b6528b085cd6b900a2c9c3_361_1.png)

8. We will now go ahead and create columns for the **Incident Date**, **Incident City**, and **Incident Plant** using the data below. Otherwise, we will use the default values.

| Column Name | Data Type | Max Number of Characters |
|---|---|---|
| Incident Date | Date and Time | N/A |
| Incident City | Single line of text | 50 |
| Incident Plant | Single line of text | 10 |

![](img/23d1402649b6528b085cd6b900a2c9c3_362_0.png)

9. Once we have completed our Content Type, it should look like the following screenshot:

![](img/23d1402649b6528b085cd6b900a2c9c3_363_0.png)

10. We now need to associate a particular InfoPath form with this Content Type. Click on Advanced Settings to launch the wizard responsible for this function.

![](img/23d1402649b6528b085cd6b900a2c9c3_363_1.png)

11. Browse to the location of your form and select it. Also, ensure that the Content Type is not set to read only.

![](img/23d1402649b6528b085cd6b900a2c9c3_363_2.png)

12. At this point, we are ready to create a new Form Library and then associate this Content Type with our newly created document library. To create a new document library, click on **Site Actions** and then **More Options**.

![](img/23d1402649b6528b085cd6b900a2c9c3_364_0.png)

13. Select **Form Library** and then provide a name and click the **Create** button.

![](img/23d1402649b6528b085cd6b900a2c9c3_364_1.png)

14. We also want to set the default template for our Form Library. We need to modify the default template setting and can do so by clicking on **Library Settings**.
