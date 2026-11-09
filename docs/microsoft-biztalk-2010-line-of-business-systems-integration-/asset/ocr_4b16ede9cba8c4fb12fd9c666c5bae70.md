![](bbox=[0.305, 0.145, 0.695, 0.4])

13. With the old, .NET Connector-based adapter one IDOC equaled one BizTalk XSD schema. This is no longer the case. The Consume Adapter Service Wizard will generate multiple schemas for each IDOC. The reason for this is the Wizard will create schemas for specific SAP types that then get imported into a Master, or Parent, Schema. If you have many IDOCs within your solution this can get very confusing unless you provide a Filename Prefix. If we provide a prefix, like CONF32, all schemas will start with this prefix, which will allow us to organize and differentiate our schemas.

14. Once we have provided this prefix, we can click the OK button and our IDOC schema will get generated.

![](bbox=[0.27, 0.6, 0.73, 0.65])

15. While this method is convenient, it comes with some baggage. When you click the OK button, it will download every version of the IDOC from your SAP system. This may leave you with way more schemas than you are interested in and may also result in timeouts from the Consume Adapter Server Wizard as there is just too much data to retrieve.