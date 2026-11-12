
In the case of the SOAP and REST APIs, by default the latest version of your Custom Objects is visible. When using the SOAP API, header values can be passed in such calls to lock a particular package version if needed (search for PackageVersionHeader in Apex Developer's Guide). Alternatively the packaged version used by these APIs can be set at an org-configuration level.

## Versioning application access through the Salesforce APIs

Under the Setup menu, search for API. You will find a setup page that allows you to download the WSDLs for one of the many Salesforce SOAP APIs. In the next section there are some versioning settings that determine which installed package version should be assumed during interactions with your packaged objects.

For example, when performing a describeSObject web service operation via the Salesforce Enterprise or Partner APIs, these APIs will return different custom fields depending on when new fields were added, as new versions of the package have been released over time. The default behavior will return all fields as the default is the current package version.

If this is important to Developer X or third-party solutions when using these Salesforce APIs, the subscriber administrator can specify an explicit previous version. If they do this, new fields available in future package versions will not be available unless this is changed. The following screenshot shows how these settings can be accessed on the API page under Setup:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_353_0.png)

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_353_1.png)

## Versioning the API functionality

When offering new versions of your application to users through the UI, new features are accessed by the user — either by opting into the feature by configuration using a new page or button, or sometimes just seamlessly such as a new type of calculation, optimization, or improvement to existing behavior with added benefits.

As the API is just another way into your application's logic, you should think about enabling these new features through the API in conceptually the same way. If it is seamlessly available through the UI to users, it becomes so in the API regardless of the API version, and thus, solutions built around your application's API will also benefit automatically without changes to the integration or extension code. However, if there is a new field or button needed in the UI, then a new API parameter or operation will be needed to enable the new feature. It is reasonable for this to be exposed in a new version of your API definition. Thus, Developer X will have to modify the existing code, test it, and deploy it before the solution can leverage it.

Trying to be sophisticated in your code by providing exactly the same behavior to older callers of your API will depend on the Service and Domain layers understanding the concept of API versus UI contexts and package versioning, which will rapidly become complex for you to develop, test, support, and document.

> Agree and communicate to your customers and partners clearly how many versions of your API you will support and give warning on retirements. Always be sure to test your APIs at different versions to ensure the lack of fields that arrive later in your package, which are not provided by older callers, does not cause errors or unwanted behavior in these cases. Try to avoid adding new required fields to your applications without a way to default them.
>
> If in your regression testing of older API versions, you notice a reduction in data volume throughput due to governors, it can be acceptable to condition newer logic (such as checks for newer fields). Again, be careful with how often you implant this type of conditional logic in your code base.

## Translation and localization

As with the guidelines around the Service layer, which can be used as a means to form your application API later in this chapter, you should ensure that any error or status messages are translated according to your use of Custom Labels, as described in Chapter 2, Leveraging Platform Features (the Apex runtime will ensure that the user locale determines the translation used).

> > It is not possible to override the default Org language used to look up the appropriate Custom Labels through Apex. However, if the API is called from a Visualforce page utilizing the language attribute as described in Chapter 8, your API code will also honor this.

Some APIs expose error codes to provide a language-neutral way to recognize a certain error use case programmatically (parsing the message text is not reliable in a translated environment). As such, one approach is to use an Apex enum and a custom Apex exception class, which captures both the translated error message and the exception code. The following enum and exception types are defined in the Application class contained within the sample code for this chapter. Note the use of the global keyword in Apex; this will be discussed later in this chapter in more detail:

```apex
global Application {
	enum ExceptionCode {
		ComplianceFailure,
		UnableToVerifyCompliance
	}
}

global virtual class ApplicationException extends Exception {
	global ExceptionCode ExceptionCode {get; private set;}

	public ApplicationException(
		ExceptionCode exceptionCode, String message) {
		this(message);
		this.exceptionCode = exceptionCode;
	}
}
```

```apex
global class ComplianceException
	extends Application.ApplicationException {
	global List<VerifyResult> failures {get; private set;}

	public ComplianceException(
		Application.ExceptionCode exceptionCode,
		String message, List<VerifyResult> failures) {
		super(exceptionCode, message);
		this.failures = failures;
	}
}
```

This code can then raise exceptions specifying the enum value related to the message as follows:

```apex
throw new ComplianceException(
    Application.ExceptionCode.ComplianceFailure,
    Label.ComplianceFailuresFound,
    failedCompliances);
```

```apex
throw new ComplianceException(
    Application.ExceptionCode.UnableToVerifyCompliance,
    Label.UnableToVerifyCompliance,
    null);
```

As with the Service layer guidelines, the API should be client agnostic, and thus you cannot assume whether the data values accepted or returned should be formatted or not. Thus, it is best to use the native Apex types for date, date/time, and number and allow the caller to decide whether or not to format the values. Returning formatted values is useful for UI callers, but for headless or automated solutions, this is not ideal.

## Terminology and platform alignment

If you're following the guidelines of the Service layer when defining your classes, methods, parameters, and interfaces (as discussed later in this chapter), then any API based on this layer should already mirror the functional terms used in your application. Having a good alignment with your application terms makes your API easier to map to functional concepts in your application and thus for Developer X to navigate around and understand and discuss with those that know the application in more functional depth.

In addition, like the Service layer, your API should support platform concerns such as bulkification and not inhibit Developer X from calling your API in a bulkified way to make optimum use of the platform resources. This also includes Developer X utilizing Apex DML statements and thus then raises the importance of bulkified Domain layer logic as well as bulkified Service layer logic.

## What are your application's integration needs?

Typically an integration need arises from a controlling process or application that consumes the functionality of your application, effectively invoking the functionality within your application automatically on behalf of a user, or as part of an event or scheduled background process. Both, the standard Salesforce APIs and any additional application APIs that you create can support this need. The following diagram shows a controlling system driving your application through either the standard Salesforce API and/or application APIs that you have provided as part of the managed package:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_357_0.png)

Although integrations are much easier when all applications are built based on the same platform, it is a good practice to have an API strategy that covers both, on- and off-platform, Developer X scenarios, ideally exposing the same functionality consistently.

### Developer X calling your APIs on-platform

A Force.com Apex developer can manipulate the data stored in your application through Apex DML statements. You can think of Apex DML as your free Apex API to your application data, as this approach avoids the need for you to build your own application APIs to purely manage record data, particularly for those Custom Objects that you are already exposing through the standard Salesforce UI.
