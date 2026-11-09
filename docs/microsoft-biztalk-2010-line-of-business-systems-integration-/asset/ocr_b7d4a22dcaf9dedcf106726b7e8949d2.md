# Vehicle maintenance lists

**Problem:** When field vehicles have maintenance work completed on them, this information needs to be recorded for both financial and regulatory purposes in the company's System of Record, which happens to be an ERP. Mobile workers want to complete this information quickly and accurately so that they can get back to actually getting "real" work done.

**Solution:** Inside of SharePoint, we can create a simple Custom List. You can think of a Custom List much like an Excel spreadsheet with the difference being, the data in a Custom List is stored within a SharePoint database. Field workers can input the required vehicle maintenance information into the Custom List without entering many keystrokes. Having these field workers logging into a complex ERP system is both time consuming and has training implications. Accessing the SharePoint List is as simple as launching a bookmarked site within Internet Explorer and leveraging Windows Authentication so that no additional credentials have to be provided.

This information needs to get to the ERP for financial and reporting purposes; BizTalk is able to consume SharePoint's Web Services on a regular basis and retrieve this information, and construct an equivalent flat file that can be loaded in the ERP system. An in depth walkthrough of this scenario is included in Chapter 8.

Some of the benefits of using this solution are:

+   - Simple user interface to input Vehicle Maintenance details
+   - No additional credentials need to be provided to application
+   - Little training required for end users since they are all familiar with the Office 2010 suite

# Field incident reporting

**Problem:** Whenever a field worker is involved in an accident or environmental event, this information must be tracked for regulatory purposes. Once again, this information needs to be captured in the organization's System of Record, which happens to be an ERP. The ERP system does allow users to complete this information via a Graphical User Interface (GUI) form within the ERP system, but requires users to complete 12 different tabs, which is very time consuming.