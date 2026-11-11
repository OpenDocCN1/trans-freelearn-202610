associative array. The if-else statement and the nested if-else statement that follows xmlrpc_decode() makes this evaluation.

The first if statement calls xmlrpc_is_fault(). This function determines if the data structure is an XML-RPC fault. If it is, the return value is an associative array with two items whose indexes are faultCode and faultString, and whose values are the responses of faultCode and faultString elements.

If it is not an error, we then check to see if it's an array or a value. Here, we are simply echoing out the values. In practice, you will want to do something like pass the arrays to another function for looping or searching.

# Creating an XML-RPC Parser Class

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
        $theRequest = "POST " . $remotePath . "HTTP/1.0\n" .
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