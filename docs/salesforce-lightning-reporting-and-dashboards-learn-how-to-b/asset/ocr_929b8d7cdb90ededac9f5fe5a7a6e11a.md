# # Hands-on exercise for working with a summary formula

Let's consider a use case: in a **Summary** report, add **Total Amount of Closed Opportunities** in all summary levels. Assuming that roll-up summary fields in Account from Opportunity called **Total Amount of Opportunities** and **Total Amount of Open Opportunities** have been created and the detail for **Total Amount of Open Opportunities** field has been discussed in the previous hands-on, here is the roll-up summary field detail for **Total Amount of Opportunities**:

![](bbox=[0.105, 0.308, 0.894, 0.683])

+   1. Create a new custom summary formula field similar to the one we created in the hands-on exercise earlier and name it `Total_Closed_Opportunities`, with this formula: `Account.Total_Amount_of_Opportunities__c:SUM - Account.Total_Amount_of_Open_Opportunities__c:SUM`