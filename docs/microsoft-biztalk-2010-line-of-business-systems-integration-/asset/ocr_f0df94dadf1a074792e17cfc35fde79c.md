14. Add the following code, which takes in the account ID value from the source schema and builds up a query statement, to the **Inline Script** window of the functoid:

```
public string BuildSFDCQuery(string accountId)
{
string query;

query = "SELECT Name, BillingStreet, BillingState, BillingCity,
BillingPostalCode, BillingCountry, PrimaryContactName__c,
PrimaryContactPhone__c FROM Account WHERE AccountNumber ='" +
accountId + "'";

return query;
}
```

15. Save the functoid's configuration and connect the functoid to the destination schema's queryString element.

16. After the **Construct** shape, add a **Send** shape (named **Send Request**) and a **Receive** (named **Receive Response**) shape. Associate the **SendRequest** shape with the **SFDC_QueryRequest** message and associate the **Receive Response** shape to the **SFDC_QueryResponse** message. Connect these shapes to the **query** operation in the orchestration port.

![](bbox=[0.43, 0.5, 0.57, 0.75])

17. Below the **Receive Response** shape, drag a **Transform** shape. Set the **Construct** shape's **Message Constructed** property to **ReimburseOutput**.