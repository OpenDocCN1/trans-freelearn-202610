In this first demonstration of this chapter, BizTalk Server will take in a message and then append some additional data elements retrieved from Salesforce.com. In this scenario, a customer of ours is requesting a reimbursement for previously purchased goods and we want to look up that customer's contact details that happen to be stored in Salesforce.com.

# Configuring the foundational BizTalk components

Our solution needs some core schemas in addition to a central orchestration containing the business process that requires access to Salesforce.com data:

+   1. Open the `Chapter10-SFDC.sln` solution and find four pre-built projects. The first is a WCF message inspector behavior that will get used in a later exercise. The second project is a component that holds a reference to the Salesforce.com session token. The next project is a simple test application that verifies our Salesforce.com credentials. The final is a fully functional BizTalk project that will be used in the last exercise.
+   2. Create a new BizTalk Server project named `Chapter10-SFDC.AccountProcessing.ReimbApproval`. Set the project's strong name key and set the Deployment Application value to **Chapter10**.
+   3. Add a new schema named `ReimbursementRequest_XML.xsd` and set the target namespace to **http://Chapter10-SFDC**.
+   4. Construct the schema as shown in the following screenshot. This schema contains details about the reimbursement request and the customer's contact information.

![](bbox=[0.395, 0.603, 0.605, 0.82])