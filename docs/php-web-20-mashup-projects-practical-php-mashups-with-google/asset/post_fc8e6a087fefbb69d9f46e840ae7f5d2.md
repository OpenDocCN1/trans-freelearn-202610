
## Passing Arrays in XML-RPC Requests

Remember that an XML-RPC array is a serialized, numerically indexed PHP array. We need to pass it as the second argument to xmlrpc_encode_request. However, if we just pass an array, xmlrpc_encode_request will create a multiple parameter XML-RPC request, like we just demonstrated. The solution is to place the numerical array into another array and pass the new bundle as the second parameter of xmlrpc_encode_request:

```
<?php
    $listArray = array("One", "Monkey", 4.3074);
    $requestMessage = xmlrpc_encode_request('theRemoteCall', $listArray);
    echo $requestMessage;
?>
```

This code will create the XML-RPC array that we need. This snippet will create the example array, we saw earlier in the chapter.

![](img/c637937d44400f1da21fb7cca101de80_43_0.png)

> This works because every parameter in an XML-RPC request is placed within its own value element. Technically, we passed only one parameter into xmlrpc_encode_request. The function recognized it as a numerical array and created the necessary structure underneath the value element.

## Passing Struct in XML-RPC Requests

An XML-RPC array is related to a struct much like a numerically indexed PHP array is to an associative PHP array. Knowing this, you can make a logical guess that to create a struct, all we need to do is change the second parameter passed in from a numerically indexed array to an associative array.

```
$listArray = array("One" => "This is a string", "Two" => "true", "This is a Name" => -98.33);
$requestMessage = xmlrpc_encode_request('theRemoteCall',
    array($listArray));
```

In this code snippet, $listArray now assigns keys to the array values before being passed to xmlrpc_encode_request. This will create the example struct, we saw earlier in the chapter.

### Creating the Request Call Header

Now that we have the body, we can create the HTTP header that the request needs. The header goes before the payload. We work backwards because one part of the header requires us to know the length of the payload.

Let's take a look at a complete XML-RPC request:

```
POST /rpc HTTP/1.0
User-Agent: XML-RPC Client
Host: www.upcdatabase.com
Content-Type: text/xml
Content-Length: 185
<?xml version="1.0" encoding="iso-8859-1"?>
<methodCall>
<methodName>lookupUPC</methodName>
<params>
    <param>
        <value><string>079400560704</string> </value>
    </param>
</params>
</methodCall>
```

Looking at this, there are several header items that can be static. Other items (that are highlighted) can be turned into variables.

- 1. After POST is the name of the script we are hitting on the server
- 2. The host
- 3. The content length
- 4. Finally, the payload

It would be efficient to create a function that would return a complete request to us. The variable candidates will be our parameters.

```
function createPostRequest($remoteServer, $remotePath, $requestBody) {
    $theRequest = "POST " . $remotePath . " HTTP/1.0\n" .
    "User-Agent: XML-RPC Client\n" .
    "Host: " . $remoteServer . "\n" .
    "Content-Type: text/xml\n" .
    "Content-Length: " . strlen($requestBody) . "\n" .
    "\n" . $requestBody . "\n";
    return $theRequest;
}
```

createPostRequest is a simple function. It takes three parameters and concatenates them with standard HTTP request headers to create a full, complete XML-RPC request. The three parameters it takes are:

- $remoteServer is the name and domain of the server. For example, "www.amazon.com". Do not add a protocol like http:// because that is not only redundant, but also illegal.
- `$remotePath` is the path to the XML-RPC service starting at the root of the server. This value starts with a beginning slash (`/`) and is the same as an absolute path in a directory structure. An example would be `"/services/xmlrpc"`.
- `$requestBody` is the part that is returned to us by `xmlrpc_encode_request`. This is the XML portion of the XML-RPC call.

This function uses `$remoteServer` and `$remotePath` to build the first line of the header and the host line. The body, `$requestBody`, is not only appended to the request, but it's size must be first analyzed by the `strlen` function for the content length part of the header.

![](img/c637937d44400f1da21fb7cca101de80_45_0.png)

> Notice how we concatenate the headers in the function. Although the headers are broken up and formatted for readability in the header output itself, there are no spaces or tabs from the end of the lines (`"\n"`) to the beginning of the next line. This is a strict formatting requirement of the HTTP protocol. If there is any whitespace at the beginning of the lines, the request will fail.

## Calling XML-RPC Using Sockets

We now have a fully formatted XML-RPC call. All variables have been serialized. Our headers have also been created. The only thing left to do is to make the actual call to the service and send it our request.

Sockets are a clean way to call XML-RPC. A socket is basically a direct network connection to another machine. We can build a socket to the XML-RPC server and send our request directly to it. This can be accomplished by another function that builds upon our `createPostRequest` function. This function will not only send the request to the server, but it will capture the result and let us manipulate it later.

```php
function send($remoteServer, $remotePort, $fullXMLRPCRequest) {
    $headers = '';
    $data = '';
    $socket = fsockopen($remoteServer, $remotePort);
    fwrite($socket, $fullXMLRPCRequest);
    while ($str = trim(fgets($socket))) {
        $headers .= $str . "\n";
    }
    while (!feof($socket)) {
        $data .= fgets($socket);
    }
    fclose($socket);
    return $data;
}
```

This function also takes the following three parameters:

- `$remoteServer` is the same exact variable you used in `createPostRequest`. Like its use in the HTTP header, do not include the protocol (http://).
- `$remotePort` is network port that you wish to connect through. For standard HTTP connections, this will be 80.
- `$fullXMLRPCRequest` is the complete XML-RPC request with headers and payload. This is the part that is returned from `createPostRequest`.

This function does a few things. Let's break it down:

After some initialization of variables, we use `fsockopen` to open our connection to the remote server.

```
$socket = fsockopen($remoteServer, $remotePort);
```

The call to `fwrite()` will send our XML-RPC request directly to the server through the socket.

```
fwrite($socket, $request);
```

A socket is basically a tunnel. We not only write through it, but we capture the server's response to it. After writing, the first thing that is returned is the HTTP response header. We don't need this information, so we can just read the response and dump it to a local variable that will be discarded when the function completes its execution.

```
while ($str = trim(fgets($socket))) {
    $headers .= $str . '\n';
}
```

After the headers come through, we capture what we're really after — the XML-RPC response payload.

```
while (!feof($socket)) {
    $data .= fgets($socket);
}
```

While data is coming through the socket, we capture everything that comes through and dump it into a local variable. Once the response data has stopped coming, we can get rid of our socket and return the portion of the data to the function caller.

```
fclose($socket);
return $data;
```

At this point, we have passed variables into a function we created, `createPostRequest()`, and it returned a full-fledged XML-RPC request. We took that request and passed it to another function that we defined, send(). This function opens a socket connection to the remote server, writes the request, and parses the response. It returns to us this XML-RPC response. We now need to transform the XML-RPC response into PHP data we can use.

## Processing an XML-RPC Response

To extract data from an XML-RPC response and transform them into PHP variables, we will use xmlrpc_encode_request()'s complementary function, xmlrpc_decode(). xmlrpc_encode_request() was smart enough to take variables, whether they are primitive variables, arrays, or structures, and transform them into an XML-RPC request.

xmlrpc_decode() will take an XML-RPC response, look for the type of data being returned for example, whether they are simple primitives, structures, or arrays, and return either a straight PHP value (if primitive) or an array of values (if they are an XML-RPC struct or array). xmlrpc_decode() will also process XML-RPC response errors. Using another function, xmlrpc_is_fault(), we can evaluate the return value to see if it is an error and handle them accordingly.

Let's create a function to demonstrate both xmlrpc_decode() and xmlrpc_is_fault().

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

This function takes an XML-RPC response as the parameter.

The first thing this function does is send the XML-RPC response through xmlrpc_decode(). If you were to examine the returned value, it would be either a simple value or an array. If the service returned just a single simple value, $data would be that value. If the return was an array, $data will be a numerically indexed PHP array. If the return was a struct or an XML-RPC fault, $data will be a PHP
