```
// extract claims from header
Dictionary<string, string> claims =
tv.GetNameValues(authorizationHeader);

// look for requested claim
string permValue = "";
    //if the required claim is missing, return false
if (!claims.TryGetValue(claimType, out permValue))
{
return false;
}

    // check for the matching claim value
if (permValue != claimValue)
{
return false;
}
    //if all checks succeed, return success
returnisValid;
}
```

9. The AfterReceiveRequest operation can now validate the token through the helper function.

```
public object AfterReceiveRequest(
ref System.ServiceModel.Channels.Message request,
IClientChannel channel,
InstanceContext instanceContext)
{
    //check claims using helper function
bool isValid = CheckValidToken();

if (isValid == false)
{
request = null;

return "access denied";
}

return null;
}
```