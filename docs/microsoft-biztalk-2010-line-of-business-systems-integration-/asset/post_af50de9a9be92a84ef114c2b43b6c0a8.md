
## Microsoft Office Integration

One of the key value adds to SharePoint and BizTalk integration is the functionality that can be leveraged from InfoPath. For instance, we can create SharePoint columns and then populate those columns using either data from InfoPath fields or data that has been populated in a BizTalk Adapter context property. Enabling this property will allow for InfoPath pre-processing instructions to be enabled. These pre-processing instructions also allow for XML messages that have been deposited in SharePoint to have an association with a form. When you click to launch the XML message that was deposited, SharePoint will find its related template and launch the form with the XML data embedded. It is important to note that as BizTalk is processing these types of messages, it does not have visibility into the form template mark up. It is strictly dealing with the underlying XML form data and does not actually receive the form template mark up.

## Template Document Library

When an XML message has been sent to a SharePoint library and we want to associate it with a particular InfoPath form, we can populate this property so that SharePoint knows where to find the matching Form template.

## Templates Namespace Column

This is the name of the SharePoint column that stores the namespace of the InfoPath solution.

## SharePoint Columns

The Windows SharePoint Services Adapter exposes properties that when populated will be inserted into the corresponding column in a SharePoint Library. For instance if we have an Approver scenario, we can create a column in the SharePoint Library and when BizTalk inserts a message into this Library, it can take data from the message body, or from the Send Port configuration, and insert that data into the SharePoint column. The column context property works much like key/value pairs. We need to insert the name of the column in the "Column XX" property and then the related value in the "Column XX Value" property. There is an upper limit of 16 columns that can be populated.

## Other Considerations

## Context Properties

Much like other adapters, the Windows SharePoint Services Adapter context properties are also available inside Orchestrations. This becomes extremely useful when building dynamic send port scenarios.

![](img/23d1402649b6528b085cd6b900a2c9c3_335_0.png)

## Windows SharePoint Services Adapter is a one-way adapter

When configuring Receive Locations or Send Ports that use the Windows SharePoint Services Adapter, we can do so using One Way Receive Locations or One Way Send Ports. What is interesting about this scenario is that we know that the Adapter leverages two way web services. This is not saying that BizTalk will be unaware of any failures that occur when SharePoint is not available or there are other runtime issues like security failures. These errors continue to get caught and reported by BizTalk. However, if you look in the Tracked service instances, within the BizTalk Administration console, you will only find one record for each connection to SharePoint.

Once again, Microsoft is abstracting a lot of the complexities involved in calling web services for us. Another benefit of Microsoft's architecture is there are no SharePoint Proxy classes that are required within our Orchestrations when we use the Windows SharePoint Services Adapter. This provides us with a loosely coupled design in our solutions. If we have projects that currently leverage the FILE Adapter, we can easily change Receive Locations or Send Ports to use the Windows SharePoint Services Adapter without recompiling our solutions. We simply need to change the Adapter Type in the BizTalk Administration Console.

## Receiving documents from SharePoint

The scenario that we are going to build is a Vacation Request Approval System using InfoPath, BizTalk, and SharePoint 2010 Foundation. An employee will fill out the form and submit it to a Forms Library in SharePoint where it will be ready for approval by the employee's supervisor. Once the vacation has been approved, BizTalk will consume this form, archive it, and then use the FILE adapter to deliver the approved Vacation Request to an ERP system. The ERP system requires this information to be in a Comma Separated Value (CSV) file.

![](img/23d1402649b6528b085cd6b900a2c9c3_336_0.png)

> Note: The examples in this chapter assume you have an existing SharePoint site in place. The following links describe how to create SharePoint sites for WSS 3.0 SP2 and SharePoint 2010 Foundation:
> 
> WSS 3.0 SP2:
> http://technet.microsoft.com/en-us/library/cc288969(office.12).aspx
> 
> SharePoint 2010 Foundation:
> http://technet.microsoft.com/en-us/library/cc263094.aspx

The foundation for this solution is a schema called `InfoPath_Vacation_Approval.xsd`. We will use this schema as a Message Type within the BizTalk solution, but we will also use this schema to build an InfoPath form. The end result is that when the Approval Form is submitted to SharePoint, and picked up by BizTalk, we will have a strongly typed message inside of BizTalk.

This schema is part of the Chapter7-ReceivingDocuments solution that is part of this book's source code.

![](img/23d1402649b6528b085cd6b900a2c9c3_337_0.png)

## Building InfoPath Form and SharePoint Forms Library

In order to design a form based upon this schema, we need to open Microsoft InfoPath Designer. This example will utilize InfoPath 2010, but this solution should also work with previous versions of the InfoPath product:

- 1. Double click on XML or Schema from Advanced Form Templates.

![](img/23d1402649b6528b085cd6b900a2c9c3_337_1.png)

- 2. Browse to the location of the **InfoPath_Vacation_Approval.xsd** schema and click the **Next** button.
- 3. When prompted with whether we want to add another XML Schema or XML Document to the main data source we want to select **No** and then click the **Finish** button.
- 4. In the right hand portion of the following screenshot, we will notice that the fields and nodes that were created in our XSD Schema are now available to be inserted on our InfoPath form:
- 5. We now have the opportunity to design the form to conform with any look and feel, or design standards that our organization may have.

![](img/23d1402649b6528b085cd6b900a2c9c3_338_0.png)

![](img/23d1402649b6528b085cd6b900a2c9c3_339_0.png)

- 6. Once we have completed the design of the form, we want to publish the form to a SharePoint Forms Library. To do this, click on File – Publish – SharePoint Server.

![](img/23d1402649b6528b085cd6b900a2c9c3_339_1.png)

- 7. Provide the URL to the SharePoint site that you would like to deploy this form to and click the Next button.

- 8. For this example, we want to choose to publish this form to a **Form Library** and then click the **Next** button.

![](img/23d1402649b6528b085cd6b900a2c9c3_340_0.png)

- 9. We now want to **Create a new form library** and then click the **Next** button.

![](img/23d1402649b6528b085cd6b900a2c9c3_340_1.png)

- 10. Provide a name for this new form library such as **Vacation Approval** and then click the **Next** button.

![](img/23d1402649b6528b085cd6b900a2c9c3_340_2.png)

- 11. Specify the **Employee Name**, **Supervisor**, and **Approval Status** as fields that we would like to see displayed in the form library.

![](img/23d1402649b6528b085cd6b900a2c9c3_341_0.png)

- 12. Publish and verify that our form library was deployed successfully. Note that we should see the **Vacation Approval** link under libraries and we should see the **Employee Name**, **Supervisor**, and **Approval Status** columns in the upper-right-hand corner of the browser.

![](img/23d1402649b6528b085cd6b900a2c9c3_341_1.png)

- 13. Click on the **Add document** link.

![](img/23d1402649b6528b085cd6b900a2c9c3_341_2.png)

- 14. The form that we previously designed gets launched.

![](img/23d1402649b6528b085cd6b900a2c9c3_342_0.png)

- 15. We now need to go back into the form and modify where the form should be submitted to after an employee has completed the form. We have a couple of options when it comes to submitting a form to a SharePoint library:
  - Using the **Submit Form** button that is part of the native InfoPath client
  - Adding a button control to the form and then attaching a **Submit Form** action to the button

- 16. We will now add the submit form functionality through the InfoPath client by clicking on **File -Info - Submit Options -To SharePoint Library**. This allows us to leverage the out-of-the-box functionality without any additional configuration or scripting.
