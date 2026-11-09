12. We are now ready to call the AC service. The result of the service call is put into a string variable for post-processing.

```
HttpResponse acRes = httpProxy.send(acReq);
String acResult = acRes.getBody();
```

13. The token that we received from the AC service does not exactly match the format needed by the Service Bus. The raw response from the AC service looks like this:

```
wrap_access_token=net.windows.servicebus.action%3dListen%252cSend%252cManage%26Issuer%3dhttps%253a%252f%252frichardseroter-sb.accesscontrol.windows.net%252f%26Audience%3dhttp%253a%252f%252frichardseroter.windows.net%26ExpiresOn%3d1290807349%26HMACSHA256%3dEcvt eRJ9T1hDo9gwWT5MWwfhOL OOR eoeCKP9esor p7JA%253d&wrap_access_token_expires_in=1200
```

14. The SB doesn't want all of that, so we need to parse out a few bits. First, remove the portion after the ampersand (wrap_access_token_expires_in), and then pull out the beginning portion (wrap_access_token=) and finally do a URL decoding. Our token needs the WRAP access_token= prefix so that the Service Bus knows what kind of authentication has been provided.

```
// clean up result
    String suffixRemoved = acResult.split('&')[0];
    String prefixRemoved = suffixRemoved.split('=')[1];
    String decodedToken = EncodingUtil.urlDecode(prefixRemoved, 'UTF-8');
    String finalToken = 'WRAP access_token="' + decodedToken + '"';
```

15. With a Service Bus-consumable token in hand, we can call the Service Bus endpoint and invoke our on-premises BizTalk service (through the REST proxy). Because we have a REST service, there is no "body" to pass in the HTTP request as all of the details are in the URL. We build up the REST URL with the account number of the customer and add an authorization header to the HTTP request.

```
String sbUrl = 'https://<your namespace>.servicebus.windows.net/DiscountService/' + myAcct.Account Number + '/Discount';
sbReq.setEndpoint(sbUrl);
sbReq.setMethod('GET');
sbReq.setHeader('Content-Type', 'text/xml');
sbReq.setHeader('Authorization', finalToken);
```