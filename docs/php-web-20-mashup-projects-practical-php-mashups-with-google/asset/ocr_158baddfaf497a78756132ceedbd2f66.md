This points us towards the types element. We'll look for a data type named CelciusToFahrenheit in there.

```
<xs:element name="CelciusToFahrenheit">
    <xs:complexType>
        <xs:sequence>
            <xs:element name="nCelcius" type="xs:decimal"/>
        </xs:sequence>
    </xs:complexType>
</xs:element>
```

Finally, we reach the exact name and type of the parameter we need to pass. We need to pass a decimal named nCelcius to an operation named CelciusToFahrenheit, in order to get the data we want.

This parameter requirement is quite simple, but common. All parameters that need to be passed by SoapClient need to be put into an associative array.

Create a PHP file on your web server that can be served. To create the parameters for this request, create an array with one element in it. The key is nCelcius while the value is the temperature you wish to convert.

```
$params = array('nCelcius' => 37);
```

# Instantiate the SoapClient

The SoapClient is an object and all calls are made through methods to this object, so we need to first instantiate it. Before we do this, we need to decide if we are going to use WSDL mode or non-WSDL mode for our SoapClient.

In WSDL mode, SoapClient gets the WSDL file from the remote server and automatically figures out which operation is available on the service, which parameters are needed, and how they should be constructed. There are also a few methods available to us in WSDL mode that allow us to query the service. For example, __getFunctions returns a list of SOAP operations available for the service, and __getTypes returns a list of SOAP data types used.

In non-WSDL mode, we specify the actual location of the web service, not the WSDL, in arguments when we instantiate. We also specify the namespace of the document at creation time. We have to be a bit more careful later when we create the actual request.

Given that WSDL will do a lot of things automatically, why would anyone ever use non-WSDL? Performance is one reason.