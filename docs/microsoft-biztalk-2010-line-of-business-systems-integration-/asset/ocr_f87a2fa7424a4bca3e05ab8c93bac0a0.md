2. Initiate the BizTalk WCF Service Deployment Wizard and create a new web service (and receive location) from the LookupDiscountRate orchestration. Complete the wizard and ensure that the service was successfully deployed to the web server. Bind the new receive location the LookupDiscountRate orchestration and start both the orchestration and corresponding receive location.

3. To create the cloud-enabled REST proxy service, return to Visual Studio 2010 and add a new website to the solution. Choose the WCF Service type and name the site Chapter10-SFDC.AccountProcessing.RestDiscountService.

4. Add a service reference named CalcServiceReference to the WCF service that we previously generated from the BizTalk project.

5. Open the IService.cs file that defines the interface for the web service. Create an object type named Discount that represents the relevant pieces of the payload that is returned by the BizTalk service.

```
[DataContract (Namespace="http://Chapter-SFDC.AccountProcessing")]
public class Discount
{
    [DataMember]
    public string AccountId { get; set; }
    [DataMember]
    public string DateDelivered { get; set; }
    [DataMember]
    public float DiscountPercentage { get; set; }
    [DataMember]
    public bool IsBestRate { get; set; }
}
```

6. Build the service contract for the IDiscountService interface that takes in an account ID and returns a Discount object. Note that we are decorating this operation with a REST attribute called WebGet that allows us to pass input values to the service through the URI.

```
[ServiceContract]
public interface IDiscountService
{
    [WebGet(UriTemplate="/{accountId}/Discount")]
    [OperationContract]
    Discount GetDiscountDetails(string accountId);
}
```

7. Open the Service.cs file and create a service class and inherit from the IDiscountService interface. Add a service reference named CalcServiceReference to the BizTalk-generated web service and insert the code needed to invoke the service.