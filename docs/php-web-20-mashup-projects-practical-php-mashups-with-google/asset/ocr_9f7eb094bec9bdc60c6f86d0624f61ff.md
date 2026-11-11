xmlrpc_set_type was designed specifically to address the presence of base64 and date/time types in XML-RPC, and their lack in PHP. The second parameter, which determines what kind of XML-RPC data type that you wish to cast, can only take a value of base64 or date/time. It is not meant to be a generic type casting function like PHP's settype.

The other structures that xmlrpc_encode_request creates, builds upon its basic handling of scalar data. This is in much the same way that XML-RPC's structs and arrays use the same basic scalar schemas at the value level. The main limitation is that xmlrpc_encode_request can accept only one parameter for its variables parameter. It works around this by accepting different array structures for the variables parameter, and outputting different XML-RPC requests depending on what it receives.

# Creating a Multiple Parameter XML-RPC Request

Sometimes, you may find a remote call that requests more than one parameter in a simple list. To do this, simply create an array of the variables and pass the array as the second argument of xmlrpc_encode_request like:

```
<?php
    $dt = '20060814T09:08:23';
    xmlrpc_set_type($dt, «datetime»);
    $listArray = array(«One», 2.09, $dt);
    $requestMessage = xmlrpc_encode_request('theRemoteCall', $listArray);
    echo $requestMessage;
?>
```

This will create an XML-RPC request with a param element for each variable. As you can see, it is perfectly legal to mix and match data types.

```
<params>
    <param>
        <value><string>One</string></value>
    </param>
    <param>
        <value><double>2.090000</double></value>
    </param>
    <param>
        <value>
<dateTime.iso8601>20060814T09:08:23</dateTime.iso8601>
        </value>
    </param>
</params>
```