
Instead, for functions requiring more than one parameter, the correct parameter order is defined by the remote function. You will have to check the API's documentation for this information and make sure you order the parameters correctly.

```
//The correct way to differentiate parameters is in their order as defined by the API
<params>
    <param><value><int>42</int></value></param>
    <param><value><int>13</int></value></param>
    <param><value><int>32</int></value></param>
</params>
```

In the request, each parameter is enclosed by a param element. Within each param, the actual parameter is wrapped up by a value element. Within this value element are the actual parameter values and data types.

## XML-RPC Data Types

XML-RPC parameters can be one of the following three types. Each is represented in a different way inside the value element.

- **Scalars:** basic primitive data types
- **Arrays:** similar to PHP numerically indexed arrays
- **Structs:** equivalent to associative arrays in the PHP world

Right now, we are using these data types and their structural definitions as request parameters. However, the same data types are used throughout XML-RPC. The server response will give us data in the same schema.

### Scalar Values

Scalar values are the common primitive data types found in most languages. Almost every scalar value must be encapsulated by an element that declares what data type that value is.

### String

This is your basic text string. It corresponds to the string data type in PHP.

```
<param>
    <value><string>Hello, world!</string></value>
</param>
```

Since this is the most common data type, any values of the value element without data type tags inside will default to string. For example, the following is perfectly legal in XML-RPC and will default to a string:

```
<param>
  <value>Goodbye, cruel world!</value>
</param>
```

### Integer

This is a four byte signed integer. It is the same as the PHP data type integer. Hence, it can take the more obvious tag of int or i4, for four byte integer.

```
<param>
  <value><int>42</int></value>
</param>
// This is the same:
<param>
  <value><i4>42</i4></value>
</param>
```

### Double

Double is a double precision signed floating point number. It is the equivalent to float in PHP. Note that in PHP 4, there was also a double data type, but this has been deprecated in preference of float.

```
<param>
  <value><double>-44.352301</double></value>
</param>
```

### Boolean

Boolean is your basic true or false state. It is the same as the PHP boolean. The difference is that in PHP, and many other languages for that matter, boolean can be represented by the keywords TRUE or FALSE, or a numerical setting of 1 for true and 0 for false. In XML-RPC, booleans can only be represented with 1 or 0.

```
<param>
    <value><boolean>0</boolean></value>
</param>
```

### Date/Time

A date/time data type specifies a date and a time value, up to the second. It follows the format YYYYMMDDTHH:MM:SS. Year, month, day, hour, minute, and second should be apparent in that format. The "T", however, is a literal. There is no date/time data type in PHP, so you will have to represent this as a string.

```
<param>
    <value>
        <dateTime.iso8601>20060710T11:30:32</dateTime.iso8601>
    </value>
</param>
```

In PHP 5, there is a date parameter of "c" that will return a date/time object in ISO 8601 format. It will not return the exact format that XML-RPC needs, but later we will use a function to automatically encode it into a date/time type.

### Base64-Encoded Binary

To transfer binary information via XML-RPC, encode it in base64 and wrap it around base64 tags.

```
<param>
    <value>
        <base64>Pj4UijBdhLr6IdvCc0Ad3NVP4OidTd8E1kRY5Edh</base64>
    </value>
</param>
```

There is no binary data type in PHP, but you can encode files in base64 for XML-RPC transfer by using the file_get_contents function.

### Arrays

Numerically indexed arrays are passed as a single structure within the value element. Arrays are defined with a specific structure of child and grandchild elements.

```
<params>
    <param>
        <value>
            <array>
                <data>
                    <value><string>One</string></value>
                    <value><string>Monkey</string></value>
                    <value><double>4.307400</double></value>
                </data>
            </array>
        </value>
    </param>
</params>
```

There are two levels of children before we actually see the data. array, which defines the value of the parameter to be an array, and data, which signals the start of the data. The data is encapsulated exactly like scalar values. There is a value element for each item in the array and they may have children elements that define the data type of the value.

Arrays can be recursive. Within each value element, can be another array as long as they contain the array/data/value descendant sequence.

### Struct

Similar to arrays, structs are the XML-RPC representation of PHP's associative arrays. Each item has a named key and a value pair. Like arrays, a single struct element defines a struct. Each item has one member element, each member element has a required name element, which names the item, and one value element which represents the value. Also, like arrays, the value element follows the definition rules of scalar values in XML-RPC.

```
<value>
    <struct>
        <member>
            <name>One</name>
            <value><string>This is a string</string></value>
        </member>
        <member>
            <name>Two</name>
            <value><boolean>1</boolean></value>
        </member>
        <member>
            <name>This is a Name</name>
            <value><double>-98.330000</double></value>
        </member>
    </struct>
</value>
```

XML-RPC requests, then, are basically HTTP POST actions that specify a remote method to be called with properly formatted parameters. Let's take a look at the response back from the server.

## XML-RPC Response

Once we make a request, we can expect one of the two types of responses from the service. If an XML-RPC request was successful, we will receive the data we requested returned to us in a fashion defined by the XML-RPC specifications. If there was an error, a special XML-RPC fault message will be returned.

Similar to a regular web page call, a header will be returned with the results in the body.

```
HTTP/1.x 200 OK
Date: Fri, 11 Aug 2007 23:34:43 GMT
Server: Apache/1.3.33 (Darwin) PHP/5.1.4 DAV/1.0.3
X-Powered-By: PHP/5.1.4
Keep-Alive: timeout=15, max=100
Connection: Keep-Alive
Content-Type: text/xml
<?xml version="1.0" encoding="iso-8859-1"?>
<methodResponse>
    <params>
        <param>
            <value><string>A woeful jeremiad</string></value>
        </param>
    </params>
</methodResponse>
```

This is a successful XML-RPC response. It should look familiar to you. methodResponse is the root element that defines this as a response. Following that is a params child. Each value that is returned is enclosed in a param element. Underneath that, each value follows the rules for scalar values we saw earlier. This example shows a single string value that the service returns. However, like the request, everything under params can also be a multiple value return, an array or a struct in addition to single values. For example, an array returned from the service would look like this:

```
<methodResponse>
    <params><param><value>
        <array>
            <data>
                <value><string>system.multicall</string></value>
                <value><string>system.listMethods</string></value>
                <value><string>system.getCapabilities</string></value>
            </data>
        </array>
    </value></param></params>
</methodResponse>
```

If the service could not fulfill your request, it will return an XML-RPC fault. Instead of a params element, methodResponse will have a fault element. methodResponse will always have either a params child or a fault child, but not both.

An XML-RPC fault is basically a struct that is returned to you. There are two named members in this struct. A faultString is a human readable alert of the error, and faultCode, which is an integer assigned by the service. Neither faultString or faultCode are defined or standardized by the XML-RPC specifications. They depend solely on the server implementation.

```
HTTP/1.x 200 OK
Date: Fri, 11 Aug 2007 23:41:18 GMT
Server: Apache/1.3.33 (Darwin) PHP/5.1.4 DAV/1.0.3
X-Powered-By: PHP/5.1.4
Keep-Alive: timeout=15, max=100
Connection: Keep-Alive
Content-Type: text/xml
<?xml version="1.0"?>
<methodResponse>
    <fault>
        <value>
            <struct>
                <member>
                    <name>faultCode</name>
                    <value><int>4</int></value>
                </member>
                <member>
                    <name>faultString</name>
                    <value><string>Too many parameters.</string></value>
                </member>
            </struct>
        </value>
    </fault>
</methodResponse>
```

## Working with XML-RPC in PHP

XML-RPC is a very straightforward and effective data transport mechanism. Requests and responses are formatted in a common way. Clients and service providers can easily implement this simple protocol. PHP has a group of XML-RPC functions that are made for working with XML-RPC data.
