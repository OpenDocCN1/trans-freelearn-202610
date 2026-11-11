
- 5. POST requests via web browsers must also have a `Content-Type` header with a value of "application/x-www-form-urlencoded". In XML-RPC, as the XML-RPC request is an XML-formatted document, it was `text/xml`.
- 6. The `Content-Length` header must have the length of the body.

HTTP requests can have many more headers and be much more complex. However, for the purposes of making REST requests, these are the only headers that we need.

![](img/c637937d44400f1da21fb7cca101de80_57_0.png)

> You may notice that all of the examples this far have been using HTTP 1.0. This may seem archaic since HTTP 1.1 has been around for years. The reason that we are using the older protocol is that with HTTP 1.1, servers can, and often do, respond with chunked responses when 1.1 is used. That is, to save response time, the server will start sending back the actual response without knowing what size it is, and thus, does not provide a Content-Length header. While most modern clients can handle this just fine, PHP does not. The nature of sockets makes it so that we must essentially handle the transfer chunks ourselves. While this is by no means impossible, it is quite tedious. Therefore, for this book, we'll stick with HTTP 1.0.

## Using Sockets to Initiate a REST Request

Knowing the differences between GET and POST, we can make a few modifications to our previous XML-RPC request functions, which uses POST, to make them work with REST. Our socket connection function from before, send(), takes a full HTTP request, opens the socket and returns the server response. We can use this exact function to do the same thing. All we need to do is create the full HTTP request. We can modify another previous function, createPostRequest() to make the HTTP request. There are some key differences from things that we will have to change, though:

- 1. We do not have the luxury of xmlrpc_encode_request() to turn PHP variables into a formatted HTTP body for us.
- 2. The variables will have to be converted into a URL query string and, for GET, appended to either after the path to the service, or, in the case of POST, appended as the body of the request.
- 3. While XML-RPC requests occur over POST, the majority of REST requests happen with GET. However, some APIs may call for POST. We should be ready to create both.

To address the first and some of the second points, we will create a function that takes an associative PHP array, loops through it, encodes the values into a URL-friendly format, and creates a single query string from the key and values.

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

## Creating GET and POST Request Functions

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

```
function createGetRequest($remoteServer, $remotePath, $paramString) {
    $theRequest = "GET " . $remotePath . "?" . $paramString . "
HTTP/1.0\n" .
    "Host: " . $remoteServer . "\n" .
    "User-Agent: XML-RPC Client\n\n";
    return $theRequest;
}
```

Looking at the first line, we see that our method now says GET instead of POST. The path is appended as normal, however, now we append a question mark to indicate that a server is going to send some query parameters. The values of the parameters are concatenated after the question mark. Content-Type, Content-Length, and the body are removed because they are not needed here.

Following functions will get a REST service response for us.

## Making a REST Parser Class

Like the XML-RPC Parser class, we should create a facade function to simplify the REST call for us. For clean code organization, we'll organize all the functions into a class.

```
class RESTParser {
    public function callService($parameters, $remoteServer, $remotePath,
    $httpMethod, $port=80) {
        $paramString = $this->makeParameterString($parameters);
        switch(strtoupper($httpMethod)) {
        case "POST":
            $fullRequest = $this->createPostRequest($remoteServer,
            $remotePath, $paramString);
            break;
        case "GET":
            $fullRequest = $this->createGetRequest($remoteServer,
            $remotePath, $paramString);
            break;
        default:
            $fullRequest = $this->createGetRequest($remoteServer,
            $remotePath, $paramString);
        }
        return $this->send($remoteServer, $port, $fullRequest);
    }
    private function makeParameterString($anArray) {
        $returnMe = '';
        foreach($anArray as $key => $value) {
            $returnMe .= "&" . $key . "=" . urlencode($value);
        }
        return substr($returnMe, 1);
    }
    private function createPostRequest($remoteServer, $remotePath,
    $paramString) {
        $theRequest = "POST " . $remotePath . " HTTP/1.0\n" .
        "Host: " . $remoteServer . "\n" .
        "User-Agent: XML-RPC Client\n" .
        "Content-Type: application/x-www-form-urlencoded\n" .
        "Content-Length: " .strlen($paramString) . "\n" .
        "\n" . $paramString . "\n";
        return $theRequest;
    }
    function createGetRequest($remoteServer, $remotePath, $paramString) {
        $theRequest = "GET " . $remotePath . "?" . $paramString . "
HTTP/1.0\n" .
        "Host: " . $remoteServer . "\n" .
        "User-Agent: XML-RPC Client\n\n";
        return $theRequest;
    }
    private function send($remoteServer, $remotePort,
    $fullXMLRPCRequest) {
        $headers = '';
        $data = '';
        $socket = fsockopen($remoteServer, $remotePort);
        fwrite($socket, $fullXMLRPCRequest);
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
}
```

This class uses the same functions as was described. Besides encapsulating the functions in a class, there is also the addition of a new callService() façade. This function is like the one in the XMLRPC Parser class with two differences. First, it does not take a remote method name as a parameter because that concept does not exist for REST. Second, the method expects the keyword of GET or POST to specify which HTTP method we want to use for the REST call.

In summary, the sequence of events that this class executes is:

- 1. Pass an array of parameters to makeParameterString to get a URL encoded query string.
- 2. Pass the results of that to either createGetRequest or createPostRequest depending on the request type we want. This will give us a full request with the proper headers.
- 3. Pass the full request, headers and all, to the send function from before. This will give us the REST response from the service.

## Testing Our REST Parser Class

To test our REST functions, we can set up a basic REST service on our web server environment. This will demonstrate how all the functions work together. The service that we will create will basically take an incoming request, report what method was used to make the request, and report any POST or GET parameters that were sent with the request.

Create a file with the following chunk of code:

```
Method <?= $_SERVER['REQUEST_METHOD'] ?>
<p>
<b>POST Variables:</b><br />
<?php foreach ($_POST as $key => $value) { ?>
    Key: <?= $key ?> | Value: <?= $value ?><br />
<?php } ?>
</p>
<p>
<b>GET Variables:</b><br />
<?php foreach ($_GET as $key => $value) { ?>
    Key: <?= $key ?> | Value: <?= $value ?><br />
<?php } ?>
</p>
```

Name this file RESTService.php and place it in an area on your web server that is accessible from a web browser.
