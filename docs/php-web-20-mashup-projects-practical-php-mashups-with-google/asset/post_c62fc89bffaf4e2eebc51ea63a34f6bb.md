
PHP's XML-RPC functions are documented at: http://www.php.net/manual/en/ref.xmlrpc.php. The collection is small but invaluable. They can be grouped into functions that enable PHP applications to be XML-RPC clients, or functions that create PHP-powered XML-RPC services. These functions are available, if PHP was compiled with the --with-xmlrpc option.

> If you check out the official PHP XML-RPC documentation, you will see they have been marked "experimental". However, they have been around since PHP 4.1 and are long considered stable and reliable for production usage.

To make your mashup, you need to know how to create an XML-RPC request, how to call the service, and how to process the return data. We will focus our attention on three PHP functions that will help us do this: xmlrpc_encode_request to convert our PHP variables into XML-RPC format, xmlrpc_decode does the reverse, and xmlrpc_is_fault checks to see if there was an error with our request to the server.

## Making an XML-RPC Request

Our first task is to create an XML-RPC request. This process can be broken up into individual steps:

- 1. Serialize the PHP data into XML-RPC format
- 2. Create an XML-RPC request by combining HTTP headers and serialized data
- 3. Make the call to the service

## Serializing Data with XML-RPC Encode Request

In network programming terms, "serializing" means transforming the data into a format that can be delivered across a network. In our case, we will serialize PHP variable values into the XML-RPC specifications that we discussed earlier. To do this, we will use the PHP function xmlrpc_encode_request. xmlrpc_encode_request will create our entire XML-RPC request call starting with the methodCall element and drilling all the way down to the value elements.

This function requires two parameters. The first is the name of the remote call we wish to request. xmlrpc_encode_request will use this to create the methodName element. The second parameter is the variables you wish to pass. It will use this to create the params structure. For this second parameter, xmlrpc_encode_request will automatically detect what kind of variable was passed into it, (variable types, numerical arrays, or associative arrays), and format them into the data type schema dictated by the XML-RPC specifications.

Let's try creating some different types of XML-RPC requests using xmlrpc_encode_request.

## Creating a Single Parameter XML-RPC Request

The simplest XML-RPC request is one single parameter. Create this simple script and put it on your web server:

```
<?php
$singleVar = "Hello!";
$requestMessage = xmlrpc_encode_request('theRemoteCall', $singleVar);
echo $requestMessage;
?>
```

Hit this page with a web browser, and examine the source. Your source will look like:

```
<?xml version="1.0" encoding="iso-8859-1"?>
<methodCall>
    <methodName>theRemoteCall</methodName>
    <params>
        <param>
            <value><string>Hello!</string></value>
        </param>
    </params>
</methodCall>
```

xmlrpc_encode_request has created the entire XML-RPC request for you.

If you manipulate the variable type that is passed, xmlrpc_encode_request is smart enough to figure out the XML-RPC data type equivalent. We can test this functionality with the following script:

```
<?php
$singleVar = "88";
settype($singleVar, "integer");
$requestMessage = xmlrpc_encode_request('theRemoteCall', $singleVar);
echo $requestMessage;
?>
```

Here, we are first assigning the value of $singleVar to be a string, whose value is "88", settype will cast $singleVar to an integer before we pass it to xmlrpc_encode_request. The resulting XML-RPC value returned by xmlrpc_encode_request will reflect this data type by wrapping it around int tags.

Again, if you hit this page with a web browser, you can view this returned structure:

```
<?xml version="1.0" encoding="iso-8859-1"?>
<methodCall>
    <methodName>theRemoteCall</methodName>
    <params>
        <param>
            <value><int>88</int></value>
        </param>
    </params>
</methodCall>
```

There are two other special data type cases that we need to be careful about.

## Double Data Type

The first case is the double data type. xmlrpc_encode_request will always pad doubles to six digits past the decimal point. For example, passing 1.6,

```
$singleVar = 1.6;
$requestMessage = xmlrpc_encode_request('theRemoteCall', $singleVar);
```

will result in the following XML-RPC snippet:

```
<value>
    <double>1.600000</double>
</value>
```

Furthermore, anything longer than six digits will be rounded. We can see how this works by passing a long number that should be rounded up, and one that should be rounded off.

```
$roundedUp = 1.123456789;
$roundedOff = 1.87654321;
$requestMessage1 = xmlrpc_encode_request('theRemoteCall', $roundedUp);
$requestMessage2 = xmlrpc_encode_request('theRemoteCall',
$roundedOff);
```

These two calls will result in these two respective value elements:

```
<value>
    <double>1.123457</double>
</value>
<value>
    <double>1.876543</double>
</value>
```

## Date/Time and Base64 Data Types

The second case that we need to take caution around is date/time and base64. Neither of them have a data type equivalent in PHP. If you assign them to a variable, PHP will consider that variable to be a string. And when they are passed to xmlrpc_encode_request, xmlrpc_encode_request will also consider them to be strings.

For example, if we had a special string of binary data, PHP can only treat that as a string of characters;

```
$singleVar = '8923hfjsd89783hiuyi9yhw938ihfasid9yh32iyr9wy';
$requestMessage = xmlrpc_encode_request('theRemoteCall', $singleVar);
```

The resulting XML-RPC request will be generated. If the remote call is specifically looking for base64, the remote call will fail.

```
<value>
    <string>8923hfjsd8938ihfasid9yh32iyr9wy</string>
</value>
```

To tell PHP they are XML-RPC data types, we need to use a special function, xmlrpc_set_type, before we call xmlrpc_encode_request. xmlrpc_set_type takes two parameters, the first is the actual variable that you wish to change, and the second is the XML-RPC type you want to change it to.

If we took the same binary string and called xmlrpc_set_type to change it to base64:

```
$singleVar = '8923hfjsd89783hiuyi9yhw938ihfasid9yh32iyr9wy';
xmlrpc_set_type($singleVar, "base64");
$requestMessage = xmlrpc_encode_request('theRemoteCall', $singleVar);
```

Our resulting XML-RPC request will be what the service is expecting.

```
<value>
    <base64>ODkyM2hmanNkODkzOGloZmFzaWQ5eWgzMml5cjl3eQ==&#10;</base64>
</value>
```

The same thing is true if you had a string formatted to the correct date/time format.

```
$singleVar = '20060814T09:08:23';
xmlrpc_set_type($singleVar, "datetime");
$requestMessage = xmlrpc_encode_request('theRemoteCall', $singleVar);
```

After encoding, the value will have the correct date/time element wrapped around it in the XML-RPC request.

```
<value>
    <dateTime.iso8601>20060814T09:08:23</dateTime.iso8601>
</value>
```

xmlrpc_set_type was designed specifically to address the presence of base64 and date/time types in XML-RPC, and their lack in PHP. The second parameter, which determines what kind of XML-RPC data type that you wish to cast, can only take a value of base64 or date/time. It is not meant to be a generic type casting function like PHP's settype.

The other structures that xmlrpc_encode_request creates, builds upon its basic handling of scalar data. This is in much the same way that XML-RPC's structs and arrays use the same basic scalar schemas at the value level. The main limitation is that xmlrpc_encode_request can accept only one parameter for its variables parameter. It works around this by accepting different array structures for the variables parameter, and outputting different XML-RPC requests depending on what it receives.

## Creating a Multiple Parameter XML-RPC Request

Sometimes, you may find a remote call that requests more than one parameter in a simple list. To do this, simply create an array of the variables and pass the array as the second argument of xmlrpc_encode_request like:

```
<?php
    $dt = '20060814T09:08:23';
    xmlrpc_set_type($dt, "datetime");
    $listArray = array("One", 2.09, $dt);
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
