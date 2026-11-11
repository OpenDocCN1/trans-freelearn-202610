```
'CultureInfo' => 'en-US',
'SafeSearch' => 'Strict',
'Flags' => '',
'Location' => '',
'Requests' => array(
    'SourceRequest' => array(
        'Source' => 'Web',
        'Offset' => 0,
        'Count' => 50,
        'ResultFields' => 'All')
    )
);
```

Again, all fields are required, but they can be blank. The Requests nesting makes it a little confusing to construct, but SoapClient handles the request admirably. After this is setup, we can go ahead and pass it to SoapClient and make the call to Search. This time, I will demonstrate the use of WSDL mode. We will wrap the call in a try-catch block to check for errors.

```
$client = new SoapClient(
    'http://soap.search.msn.com/webservices.asmx?wsdl',
    array('trace' => true);
    );

try{
    $result = $client->Search($params);
}
catch(SoapFault $e) {
    echo "Error!<br>";
    echo "faultcode: " . $e->faultcode . "<br>";
    echo "faultstring: " . $e->faultstring. "<br>";
    echo "detail: " . $e->detail;
}
```

When it comes back, we can break down the $result variable to see how we're going to parse the response. Looking at var_dump results and documentation, we see the $result is an object of the Response class. There is a property that is an object named Responses. Responses has a property named SourceResponse. SourceResponse holds information about the query results. The actual query results are in an array named Results, which is one of the properties of SourceResponse. The elements of the Results array are objects named Result. The properties of the Result object are the Title, Description, Actual URL, Display URL, and a URL to a cached version.