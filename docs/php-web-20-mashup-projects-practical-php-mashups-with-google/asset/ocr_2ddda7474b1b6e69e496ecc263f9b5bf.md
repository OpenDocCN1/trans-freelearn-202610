# Date/Time and Base64 Data Types

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