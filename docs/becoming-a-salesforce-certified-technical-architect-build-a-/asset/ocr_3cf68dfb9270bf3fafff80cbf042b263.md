| Object | Expected number of records | Is it an LDV? | Mitigation strategy |
|---|---|---|---|
| Quote | A quote is generated for each new online signup process. It is assumed that there will be less than one million records per year. | No. | N/A |
| Opportunity | The Opportunity object is only used for B2B deals. It is assumed that there will be less than 500,000 records per year. | No. | N/A |
| Case | The company has an average of three cases per customer per year. This is nearly 20 million records, not including other types of cases (such as reporting a failure). | Yes. | All cases more than a year old should be archived using Salesforce Big Objects. |
| AccountContactRelation | This object will be heavily utilized to control record visibility. You can assume more than 10 million records (based on 8.5 million properties and 6.7 million customers; some are related to more than one property). | Yes. | This object is tightly related to Account (Person account, property) and Contact objects. It should be archived in a similar way to these objects.
The AccountContactRelation object will be archived using Big Objects (for the sake of data integrity only, as it will not be driving any record visibility anymore). |

Table 15.1 – Main objects and identified LDVs