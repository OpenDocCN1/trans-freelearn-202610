# <negotiate>

For the specific web script response format, the accept header MIME type can be provided using this optional element. There can be multiple entries of this element in the description document. It is mandatory to provide the value for this tag. The value will hold the type of response format. Also, it is mandatory to provide the accept attribute that mentions the MIME type.

```
```xml
<negotiate accept="text/html">html</negotiate>
<negotiate accept="text/xml">xml</negotiate>
```
```

With the definition of one negotiate element in the description document, content negotiation is enabled.

> It is interesting to understand how the negotiate and format elements are correlated. Each web script returns result in a response stream and it might be encoded based on the MIME type. If during invoking a web script, any hint is not provided on the MIME type, then the web script framework will use its predefined encoding. While invoking a web script, it is possible to provide the hint to let the script framework know about the MIME type for response. Using the format element, as a URL extension or as a format argument, the response format can be specified, which is mapped to a MIME type in the web script framework. Another option is to specify the Accept header, which can be done using the negotiate element.

# <lifecycle>

The lifecycle element is to describe the lifecycle stage of a web script. This element can be specified in the description document as follows:

```
```xml
<lifecycle>sample</lifecycle>
```
```

Web scripts in their development lifecycle can initially be in draft state, then are fully used in the production mode, and at the end are deprecated when they are not used anymore. These stages can be specified for web scripts using this element. Different possible values for this element are shown in the subsequent sections.

# none

This indicates that the web script is not part of any lifecycle.

# sample

This mentions that the web script is a sample web script and should not be used for production use.