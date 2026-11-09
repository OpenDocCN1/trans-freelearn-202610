3. Our Receive Port, called **RetrieveSharePointGUIDs**, has one Receive Location called **RetrieveSharePointGUIDsFromSQL**. The Receive Location will use the WCF-Custom Adapter, the **BizTalkServerApplication** Host Instance, and the **XMLReceive** pipeline.

![](bbox=[0.23, 0.23, 0.77, 0.56])

> Note: We can also use the WCF-SQL Adapter in this solution as well. Since the Consume Adapter Service Wizard specifies WCF-Custom Adapter in the binding file that it automatically generates we will use it. Essentially, both adapters are leveraging the sqlBinding so the underlying functionality is the same.

4. Within this Receive Location, we need to provide an **Address (URI)**. In this case, we can provide the following: `mssql://localhost//SharePointList?InboundId=1`. **Localhost** represents the Database Server hosting the **SharePointList** database. We also have to provide a unique **InboundId** for this connection in the event we have multiple polling Receive Locations configured to use this same database. For more information regarding the **InboundId** property, please refer to Chapter 2.