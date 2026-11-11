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