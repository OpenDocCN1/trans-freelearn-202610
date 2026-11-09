
14. Like before, our public constructor will accept the values needed to set our member variables:

```
public ClaimVerificationBehavior(
string serviceNamespace,
string trustedTokenPolicyKey,
string acsHostName,
string trustedAudience,
string claimType,
string claimValue)
{
this.serviceNamespace = serviceNamespace;
this.trustedTokenPolicyKey = trustedTokenPolicyKey;
this.acsHostName = acsHostName;
this.trustedAudience = trustedAudience;
this.claimType = claimType;
this.claimValue = claimValue;
}
```

15. The final part of the behavior is the interface-required ApplyDispatchBehavior operation which adds our ClaimInspector object to the list of message inspectors that WCF applies upon message receipt.

```
public void AddBindingParameters(ServiceEndpoint endpoint,
System.ServiceModel.Channels.BindingParameterCollection
bindingParameters)
{}

public void ApplyClientBehavior(ServiceEndpoint endpoint, System.
ServiceModel.Dispatcher.ClientRuntime clientRuntime)
{}

public void ApplyDispatchBehavior(ServiceEndpoint endpoint,
System.ServiceModel.Dispatcher.EndpointDispatcher
endpointDispatcher)
{
endpointDispatcher.DispatchRuntime.MessageInspectors.Add(new ClaimInspector(serviceNamespace, trustedTokenPolicyKey, acsHostName, trustedAudience, claimType, claimValue));
}

public void Validate(ServiceEndpoint endpoint)
{}
```

16. The final piece of this puzzle is the object that allows us to configure this behavior as part of a configuration binding. Create a new class named ClaimBehaviorExtension.cs.

17. Add the following three "using" statements to the class:

```
using System.ServiceModel;
using System.ServiceModel.Configuration; //for extension element
using System.Configuration; //for configurationproperty
```

18. This public class should inherit from the WCF BehaviorExtensionElement interface.

19. Create a series of public properties that we can set on our service configuration.

```
//default constructor
public ClaimBehaviorExtension() {}

[ConfigurationProperty("serviceNamespace", IsRequired=true)]
public string ServiceNamespace
{
    get { return base["serviceNamespace"].ToString(); }
    set { base["serviceNamespace"] = value; }
}

[ConfigurationProperty("trustedTokenPolicyKey", IsRequired=true)]
public string TrustedTokenPolicyKey
{
    get { return base["trustedTokenPolicyKey"].ToString(); }
    set { base["trustedTokenPolicyKey"] = value; }
}

[ConfigurationProperty("acsHostName", IsRequired=true)]
public string AcsHostName
{
    get { return base["acsHostName"].ToString(); }
    set { base["acsHostName"] = value; }
}

[ConfigurationProperty("trustedAudience", IsRequired=true)]
public string TrustedAudience
{
    get { return base["trustedAudience"].ToString(); }
    set { base["trustedAudience"] = value; }
}

[ConfigurationProperty("claimType", IsRequired=true)]
public string ClaimType
{
get { return base["claimType"].ToString(); }
set { base["claimType"] = value; }
}

[ConfigurationProperty("claimValue", IsRequired=true)]
public string ClaimValue
{
    get { return base["claimValue"].ToString(); }
    set { base["claimValue"] = value; }
}
```

20. Next, set the BehaviorType property to the .NET type of the actual custom behavior class.

```
public override Type BehaviorType
{
    get
    {
        return typeof(ClaimVerificationBehavior);
    }
}
```

21. Finally, describe the CreateBehavior operation that instantiates our custom claims inspector behavior and passes in all the values that the service defines in its configuration settings.

```
protected override object CreateBehavior()
{
    return new ClaimVerificationBehavior(
        ServiceNamespace,
        TrustedTokenPolicyKey,
        AcsHostName,
        TrustedAudience,
        ClaimType,
        ClaimValue);
}
```

At this point, we have our claims defined in the AppFabric AC and a new WCF behavior that can validate claims. Up next, configuring BizTalk Server 2010 to use our custom behavior and validate the incoming service calls.

## Configuring the BizTalk Service to verify claims

BizTalk Server 2010 has clear extension points for WCF and can leverage service and endpoint behaviors in its processing pipeline.

- 1. Sign the custom WCF behavior with a strong name key and add it to the Global Assembly Cache (GAC) of the server(s) with BizTalk Server installed.
- 2. For BizTalk Server to find custom behaviors, they must be added to the machine.config file of the server(s). On my server, this file is located at `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config`. If you are running a 64 bit machine, don't forget to also open and add to the 64-bit machine.config file as well (`C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config`).
- 3. In the `behaviorextensions` section of the `machine.config` file(s), add the following block (changing the public key token per your signing key).
- 4. If you have the BizTalk Server Administration Console open, close, then reopen it to avoid any caching problems.
- 5. Browse to the receive location for the **RequestAccessService**.
- 6. Visit the **Behaviors** tab under the adapter configuration and add a new **EndpointBehavior**. Our **ClaimValidationBehavior** should be listed and available for selection.

```
<add name="ClaimValidationBehavior" type="Chapter6.
ClaimValidationBehavior.ClaimBehaviorExtension, Chapter6.
ClaimValidationBehavior, Version=1.0.0.0, Culture=neutral,
PublicKeyToken=<your public key>"/>
```

![](img/23d1402649b6528b085cd6b900a2c9c3_296_0.png)

7. Once we add the behavior to the receive location configuration, we can view the exposed properties and set them per our AppFabric AC values.

| Property | Value | Comment |
|---|---|---|
| acsHostName | Accesscontrol.windows.net |  |
| claimType | ActivityPermission | This is from the AC output claim |
| claimValue | Create | Also from the output claim |
| serviceNamespace | richardseroter | Use the value of your AppFabric namespace |
| trustedAudience | http://localhost/BuildingAccessManagementService | From the AC scope "appliesTo" value |
| trustedTokenPolicyKey | <insert your token policy key> | From the AC TokenPolicy |

8. Save the receive location configuration.

We have now configured BizTalk Server 2010 to validate AppFabric AC claims. Notice that we have yet to extend our endpoints to the cloud, but, we can still use the claims even for on-premises-to-on-premises scenarios.

## Building the service consumer

To test our configuration we need a valid service consumer. Within the Visual Studio 2010 starter project provided for this chapter, you should find a Windows Forms project named Chapter6.ServiceConsumer.WinUI.

- 1. Open the Chapter6.ServiceConsumer.WinUI project and add a service reference to http://localhost/RequestAccessService/RequestAccessService.svc.
- 2. Add project references to System.Web, System.ServiceModel, and System.ServiceModel.Web.
- 3. Add the following "using" statements to the code of the form:

```
using System.ServiceModel;
using System.ServiceModel.Web;
using System.Web;
using System.Net;    //for HttpWebRequest
using System.IO;    //for Streaming
```

4. Add a helper function named BuildToken to the form's code. Tokens are requested in a few ways, and I will demonstrate here how to do a simple, yet effective, request.

```
private string BuildToken()
{
    //get the scope resource from the form
    string wrap_scope = HttpUtility.UrlEncode(txtScope.Text);
    //get the AC issuer name from the form
    string wrap_name = txtIssuerName.Text;
    //get the AC issuer key from the form
    string wrap_password =
HttpUtility.UrlEncode(txtIssuerKey.Text);

    //build token request
    //NOTE: if you don't pass in the correct (or any) input claims,
    get HTTP 400 error (bad request)
    string tokenReq = "wrap_scope=" + wrap_scope + "&wrap_name=" +
    wrap_name + "&wrap_password=" + wrap_password +
    "&Email=riley@hotmail.com&Group="+ cbSubmitterType.SelectedItem;

    //create web request to AC endpoint
    HttpWebRequest req =
    (HttpWebRequest)WebRequest.Create(
    "https://your namespace.accesscontrol.windows.net/WRAPv0.9");
    req.ContentType = "application/x-www-form-urlencoded";
    req.Method = "POST";

    //stream the token request string
    using (Stream reqStream = req.GetRequestStream())
    {
        var bytes = Encoding.UTF8.GetBytes(tokenReq);
        reqStream.Write(bytes, 0, bytes.Length);
        reqStream.Close();
    }

    //get the token result
    var response = (HttpWebResponse)req.GetResponse();
    var token = new StreamReader(response.GetResponseStream()).
    ReadToEnd();

    //remove the "wrap_access_token" prefix
    string splitToken = token.Split('&').Single(value => value.
    StartsWith("wrap_access_token=", StringComparison.
    OrdinalIgnoreCase)).Split('=')[1];
    string tokenHeader = HttpUtility.UrlDecode(splitToken);

    return tokenHeader;
}
```
