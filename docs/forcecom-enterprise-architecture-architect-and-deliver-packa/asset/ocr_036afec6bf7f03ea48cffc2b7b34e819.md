# Versioning Apex REST application APIs

The generally recognized way in which REST APIs are versioned is to include the version in the URI. Notice that v30.0 (the Salesforce platform version) was included in the standard Salesforce REST API example shown earlier, but no version was included in the URI for the REST API exposing the compliance report operation. The platform does not provide a facility for versioning Apex REST APIs in the same way as its own APIs.

# Behavior versioning

The Salesforce REST API, and any Apex REST APIs exposed, can receive from a new HTTP header, such as x-sfdc-packageVersion-fforce: 1.10. In the case of Apex REST APIs, this can be used to allow the packaged API code to implement behavior versioning. This can be used to set the version returned by System.requestVersion as described earlier in this chapter.

This header is optional; without it, the current package version installed is assumed. My preceding recommendations around the utilization of behavior versioning also apply here. Because the header is optional, its usefulness is diluted.

# Definition versioning

The preceding HTTP header does not affect the visibility of the API's definition in the same way that the platform does for Apex API's as illustrated earlier in the chapter. One option to address this is to map a versioned URI to a specific Apex resource class per version, for example, to support version v1.0 and v2.0 of the compliance REST API. This approach allows you to expose URI's with your version included:

```
/service/apexrest/fforce/v1.0/compliance
/service/apexrest/fforce/v2.0/compliance
```

When versioning the API definition, you would need to also capture copies of the Apex data types used in the Service layer. This would isolate each REST API version from underlying changes of the Service layer as it evolves. The main coding overhead here is going to be marshaling between the Apex types defined in the Service layer (which are always the latest version) and those in the class implementing the REST API.