| Element Name | Purpose |
|---|---|
| LDATE | Logical date/actual date of the confirmation. In our situation we are just going to provide the current date. |
| BUDAT | Posting date of the confirmation. In our situation we are just going to provide the current date. |
| LTIME | Logical time/actual time of the confirmation. In our situation we are just going to provide the current time. |
| ERTIME | Entry time of the confirmation. In our situation, we are just going to provide the current time. |
| ARBPL | Work center |
| AUFNR | This field is where the Order Number for the job is submitted. |
| VORNR | Order Numbers, usually have an Operation associated with them so we can leverage this field to provide the Operation Number. |
| ISMN W | The duration of the work that was performed. |

Finally, a GUID element exists that associates itself with the SAP transaction ID (TID). We can provide our own GUID, or let the adapter provide a GUID.

![](bbox=[0.38, 0.46, 0.62, 0.57])

# Building a BizTalk application – Sending IDOC

The application that we are going to build will receive Vehicle Timesheets from our Work Order Management system, transform the data and construct a CONF32 Confirmation IDOC. You may be asking yourself "Vehicles have Timesheets"? Yes they do! This scenario is an example of a real world solution where we have employees in the Energy sector out working on the maintenance of energy infrastructure. Much like we would expect to track the time that an employee spends on a job, we also want to track the time of equipment that has been used. A Vehicle makes up a significant portion of the overall costs on the job and therefore we want to ensure these costs are recorded in SAP. The, out-of-the-box, CONF32 IDOC allows us to send this information to SAP without customization.