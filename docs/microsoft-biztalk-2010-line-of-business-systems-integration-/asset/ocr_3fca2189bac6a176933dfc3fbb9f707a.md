![](bbox=[0.22, 0.15, 0.78, 0.4])

2. Add the record ID to the ID field of the generated instance of a Customer_XML message.

```
<ns0:Customer xmlns:ns0="http://Chapter3-DynamicsCRM.AccountInt.Customer_XML">
  <ID>C210E386-D58A-E011-A552-000C29D4FA55</ID>
  <Name>
    <Prefix />
    <FirstName />
    <MiddleName />
    <LastName />
  </Name>
  <Addresses>
```

3. Copy the XML instance file to the folder being polled by the BizTalk receive location.