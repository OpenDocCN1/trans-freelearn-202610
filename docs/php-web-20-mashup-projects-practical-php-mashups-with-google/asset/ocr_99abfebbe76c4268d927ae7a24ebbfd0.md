We do some object creation here. XML-RPC, REST, and Amazon XML parsers are created. The incoming UPC code is in a field named numberSubmitted. As we'll be checking and using this later, we set it to a local variable named $submitted.

```
switch ($submitted) {
    case strlen($submitted) == 12:
        $upcDbCall = "lookupUPC";
        break;
    case strlen($submitted) == 13:
        $upcDbCall = "lookupEAN";
        break;
    default:
        $upcDbCall = '';
}
```

We now set up our first call to the Internet UPC Database service. The service has two available calls. lookupUPC is used to look up product information based on UPC codes. lookupEAN is used to look up product information based on EAN. If a 12-digit code was submitted, we will assume that it was a UPC code and use lookupUPC. If a 13-digit code was submitted, we will assume that it was a EAN code and use lookupEAN. This switch statement detects the length of the submitted code and stores the name of the service that we will use in the variable $upcDbCall.

```
$returnedXMLRPC = $XMLRPCparser->callService($upcDbCall,
    $submitted,
    'www.upcdatabase.com',
    '/rpc',
    80);
```

After the setup, we call the UPC Database web service, passing the name of the call that we wish to use (lookupEAN or lookupUPC) and the submitted code. The rest of the parameters, domain, path, and port, are required values for the UPC Database service. If you recall, the callService method returns the values that the Database service in the form of an array. We catch this array in $returnedXMLRPC. On a successful lookup, one of the elements returned, regardless of whether we used lookupEAN or lookupUPC, is an element named upc, which is the UPC code of the item.

```
$restParams = array();
$restParams['Service'] = "AWSCommerceService";
$restParams['AWSAccessKeyId'] = 'Your Amazon Access ID Key';
$restParams['Operation'] = 'ItemLookup';
$restParams['XMLEscaping'] = 'Single';
$restParams['IdType'] = 'UPC';
$restParams['ResponseGroup'] = 'Large';
$restParams['ItemId'] = $returnedXMLRPC['upc'];
$restParams['SearchIndex'] = $_POST['SearchIndex'];
```