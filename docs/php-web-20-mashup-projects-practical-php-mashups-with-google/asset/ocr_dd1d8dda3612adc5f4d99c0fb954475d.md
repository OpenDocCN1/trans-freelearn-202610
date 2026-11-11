```
function makeParameterString($anArray) {
    $returnMe = '';
    foreach($anArray as $key => $value) {
        $returnMe .= "&" . $key . "=" . urlencode($value);
    }
    return substr($returnMe, 1);
}
```

This function assumes that the keys in the array are the names of the parameters and the values of the array are the values of the variables that you want to pass. The foreach loop in this function goes through the array and formats the name and value pair of each array item into a query string. Each name/value pair is separated with an ampersand, so we start off with that first. We concatenate the name of the parameter, $key, to the string, followed by an equal sign to assign the value, and finally the value of the parameter represented by $value. The value is passed through the urlencode() function before it is concatenated. Outside of the loop, we strip out the leading ampersand of the first name/value pair with substr() and return it.

# Creating GET and POST Request Functions

Now that we have the parameter string, we can create the headers and make a request out of it. We'll start off with a POST request because it is strikingly similar to the XML-RPC version.

```
function createPostRequest($remoteServer, $remotePath, $paramString) {
    $theRequest = "POST " . $remotePath . " HTTP/1.0\n" .
    "User-Agent: XML-RPC Client\n" .
    "Host: " . $remoteServer . "\n" .
    "Content-Type: application/x-www-form-urlencoded\n" .
    "Content-Length: " . strlen($paramString) . "\n" .
    "\n" . $paramString . "\n";
    return $theRequest;
}
```

This function takes a server address, a path to the service, and a parameter string to make a full HTTP POST request. The only difference is that the Content-Type header has been changed from text/xml, which is what an XML-RPC request is, to application/x-www-form-urlencoded, which is a more generic POST type.

Now, we can create a function to make a GET request. This should be very similar to the POST request, but the parameters need to be moved up to the first line, the method changed to GET, and the Content-Length changed to zero, because there is no body.