The only thing you may run across in the course of making a mashup, especially with proprietary data, is the use of SOAP headers. SOAP headers often contain information about the transaction itself. One of the common uses for headers is authentication data. You may have to supply some credentials to the service before it fulfills your request. In the WSDL, a header requirement is defined in the input element here in the bindings. Like the messages definition, it will state the data type required with the part attribute.

```
<input>
    <soap:header message="tns:submitPassword" part=
        "xsd:string" use="literal"/>
    <soap:body
```

Then, you can trace back up the WSDL to the message section to find out the exact element name you need to pass in the SOAP header.

```
<message name="submitPassword">
    <part name="password_header" element="Password" />
```

The element attribute is the key. It states the name of the element you need to pass in the SOAP header. In this example, this service expects a SOAP header with an element of Password that is a string to be passed with the SOAP message.

We won't run across headers in the Google and Microsoft Live Search APIs, but be aware of them if you do run across one in the WSDL.

# service Element

Lastly, we come to the service element. This element gives us the specific location of where the SOAP action point is for each port. Each port will have its own element here, followed by the SOAP address tag that points back to the service. Google's service element looks like this:

```
<service name="GoogleSearchService">
    <port name="GoogleSearchPort" binding="typens:
    GoogleSearchBinding">
        <soap:address location="http://api.google.com/search/beta2"/>
    </port>
</service>
```

The main thing this tells us is that all operations that happen in GoogleSearchPort occur at the URL http://api.google.com/search/beta2. We may need to use this information later if we hit a service directly.