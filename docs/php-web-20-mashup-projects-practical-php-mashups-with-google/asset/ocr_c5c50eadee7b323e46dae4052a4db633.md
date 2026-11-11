that request and passed it to another function that we defined, send(). This function opens a socket connection to the remote server, writes the request, and parses the response. It returns to us this XML-RPC response. We now need to transform the XML-RPC response into PHP data we can use.

# Processing an XML-RPC Response

To extract data from an XML-RPC response and transform them into PHP variables, we will use xmlrpc_encode_request() 's complementary function, xmlrpc_decode(). xmlrpc_encode_request() was smart enough to take variables, whether they are primitive variables, arrays, or structures, and transform them into an XML-RPC request.

xmlrpc_decode() will take an XML-RPC response, look for the type of data being returned for example, whether they are simple primitives, structures, or arrays, and return either a straight PHP value (if primitive) or an array of values (if they are an XML-RPC struct or array). xmlrpc_decode() will also process XML-RPC response errors. Using another function, xmlrpc_is_fault(), we can evaluate the return value to see if it is an error and handle them accordingly.

Let's create a function to demonstrate both xmlrpc_decode() and xmlrpc_is_fault().

```
```php
function processXMLRPCResponse($xmlrpcResponse) {
    $data = xmlrpc_decode($xmlrpcResponse);
    if (xmlrpc_is_fault($data)) {
        echo 'Error Code: ' . $data['faultCode'] . "\n";
        echo 'Error Message: ' . $data['faultString'];
    } else {
        if (is_array($data)) {
            var_dump($data);
        } else {
            echo $data;
        }
    }
}
```
```

This function takes an XML-RPC response as the parameter.

The first thing this function does is send the XML-RPC response through xmlrpc_decode(). If you were to examine the returned value, it would be either a simple value or an array. If the service returned just a single simple value, $data would be that value. If the return was an array, $data will be a numerically indexed PHP array. If the return was a struct or an XML-RPC fault, $data will be a PHP