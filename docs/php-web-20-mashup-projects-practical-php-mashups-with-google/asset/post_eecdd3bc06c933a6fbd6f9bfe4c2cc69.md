
associative array. The if-else statement and the nested if-else statement that follows xmlrpc_decode() makes this evaluation.

The first if statement calls xmlrpc_is_fault(). This function determines if the data structure is an XML-RPC fault. If it is, the return value is an associative array with two items whose indexes are faultCode and faultString, and whose values are the responses of faultCode and faultString elements.

If it is not an error, we then check to see if it's an array or a value. Here, we are simply echoing out the values. In practice, you will want to do something like pass the arrays to another function for looping or searching.

## Creating an XML-RPC Parser Class

Using all three functions (createPostRequest, send, and processXMLRPCResponse), we have some basic tools to create an all-purpose XML-RPC parser utility class. To make it even simpler to use these functions, we can add a public facade function to call the others.

```
class XMLRPCParser {
    public function callService($remoteMethod, $parameters,
    $remoteServer, $remotePath, $port=80) {
        $requestXML = xmlrpc_encode_request($remoteMethod, $parameters);
        $fullRequest = $this->createPostRequest($remoteServer,
        $remotePath, $requestXML);
        $response = $this->send($remoteServer, $port, $fullRequest);
        return xmlrpc_decode($response);
    }

    private function createPostRequest($remoteServer, $remotePath,
    $requestBody) {
        $theRequest = "POST " . $remotePath . " HTTP/1.0\n" .
        "Host: " . $remoteServer . "\n" .
        "User-Agent: XML-RPC Client\n" .
        "Content-Type: text/xml\n" .
        "Content-Length: " . strlen($requestBody) . "\n" .
        "\n" . $requestBody . "\n";
        return $theRequest;
    }

    private function send($remoteServer, $remotePort,
    $fullXMLRPCRequest) {
        $headers = '';
        $data = '';
        $socket = fsockopen($remoteServer, $remotePort);
        fwrite($socket, $fullXMLRPCRequest);
```

```
while ($str = trim(fgets($socket))) {
    $headers .= $str . '\n';
}

$data = '';
while (!feof($socket)) {
    $data .= fgets($socket);
}
fclose($socket);
return $data;
}
public function processXMLRPCResponse($data) {
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
}
```

We've seen the last three functions. The first function, callService(), is a façade for the xmlrpc_encode_request(), createPostRequest(), and send() functions. Any of our applications that wants to make an XML-RPC call can just use callService() to handle everything. We pass it as the remote method we want to call, the parameters as an array, the remote server name, the path to the service, and, if necessary, the port number. The first line in the function takes the name of the method and the parameters and uses xmlrpc_encode_request() to get a XML-RPC request. We pass the request along with the server name, and the service path to createPostRequest(). This returns to us the XML-RPC request and a valid HTTP POST header. Finally, we pass that to send() which makes a socket connection to the server and gives it the XML-RPC request. The return value of send() is the XML-RPC response.

## Testing Our XML-RPC Parser Class

To see this all in action, we can use it to call a few public XML-RPC services. In the example code, the file exampleRPC.php has the XML-RPC parser class plus a few lines of code at the top to call a few services:

```php
$parser = new XMLRPCParser();
echo "<h3>First Example</h3>";
$returnedData = $parser->callService('latestDownloadURL',
    null,
    'www.upcdatabase.com',
    '/rpc',
    80);
$parser->processXMLRPCResponse($returnedData);
echo "<h3>Second Example</h3>";
$returnedData = $parser->callService('geocode',
    '1000 Elysian Park Ave., Los Angeles, CA',
    'geocoder.us',
    '/service/xmlrpc',
    80);
$parser->processXMLRPCResponse($returnedData);

class XMLRPCParser {
/* Rest of XMLRPCParser Class */
}
```

In the first line, we create the XML-RPC Parser object, which will be used twice. In the first example, we will use the Internet UPC Database XML-RPC service to call a method named latestDownloadURL. This returns the current URL to a zipped copy of the entire Internet UPC Database. The service is located at http://www.upcdatabase.com/rpc. We will use it to look up product information before we send it to Amazon, later in our mashup.

This service takes no parameters. You will run into a lot of XML-RPC methods that do not take any parameters. However, xmlrpc_encode_request() requires something to be passed for the parameters in the second argument. In such a case, simply pass null for the second argument to xmlrpc_encode_request() like we do here. In this example, null eventually gets passed to xmlrpc_encode_request() in the first line of the callService() function.

In the second example, we use the geocoder at http://geocoder.us/service/xmlrpc. Geocoder takes an address and finds the longitude and latitude of the address. The remote method is called geocode and it takes a string of the address that you want to look up. In both examples, we pass the results of callService() to processXMLRPCResponse(), which simply displays the results from the service call.

Now we are ready to see this in action. Save this file to your web server and load the page in your browser.

You should see something like this:

![](img/c637937d44400f1da21fb7cca101de80_51_0.png)

In the first example, the return is just a simple string, processXMLRPCResponse() just outputs this on the browser. In the geocoder.us example, an array is returned by the response. processXMLRPCResponse() passes arrays through var_dump(), which outputs the key, value, and value data type for each item in the array.

Satisfied that this class works, we can use this class for our mashup. Remember to remove the example instantiation and usage code before actually using this class.

## Using PEAR to Handle XML-RPC

Out of the box, PHP does a good job of handling XML-RPC. There are a few drawbacks with our approach. The first is that it does require PHP to have been compiled with -with-xmlrpc option. If not, you will have to recompile PHP. If you are in a tightly controlled corporate environment, a shared web hosting service, or using a precompiled binary, recompiling may not be an option to you.

From a development effort standpoint, our approach is a bit of a hassle. xmlrpc_encode_request and xmlrpc_decode do a lot of translation work for us, but we still need to make the HTTP headers and get down and dirty with sockets in order to make a tunnel to the server. These two tasks seem like they can be abstracted out to another function. Fortunately, PEAR has an XML-RPC extension that does both of these things for us.

PEAR, if you are not familiar with it, is an official set of third-party libraries compiled for PHP. Although, it is its own separate open-source project, the PEAR installer and package manager is included with the official PHP release. Using the PEAR installer, you can easily install, upgrade, and uninstall PEAR libraries with simple command line options.

To see what you currently have installed, go into your command line on the PHP server and type in the command to list your installed PEAR extensions:

```
Buttercup:~ root# pear list
```

Depending on how PHP was installed, you might have to be the root user to manage PEAR libraries on the machine. Another problem is that your PHP might be non-standards, and it may not have PEAR installed. If this is the case, you can download PEAR from http://pear.php.net/package/PEAR/download.

Assuming you have the correct permissions and PEAR is installed, you may see something like this:

```
Buttercup:~ root# pear list
Installed packages:
====================
Package Version State
Archive_Tar 1.1 stable
Console_Getopt 1.2 stable
...
PEAR 1.3.6 stable
XML_Parser 1.0.1 stable
XML_RPC 1.4.0 stable
```

These are your currently installed PEAR extensions. If you see the last line, XML_RPC, you are in business. PEAR's XML-RPC extension is already installed and you can go ahead and use it. If an XML_RPC line is not on the output list, you can simply install the extension by typing:

```
Buttercup:~ root# pear install XML-RPC
```

Let's take a quick look at how the XML_RPC extension is used to interact with XML-RPC services. Let's create a simple script to invoke a service from the UPC Internet Database:

```
<?php
require_once("XML/RPC.php");
$params = array (new XML_RPC_VALUE('079400560704', 'string'));
$msg = new XML_RPC_Message('lookupUPC', $params);
$client = new XML_RPC_Client('/rpc', 'www.upcdatabase.com');
$retVal = $client->send($msg);
$valueObj = $retVal->value();
echo $valueObj->scalarVal();
?>
```
