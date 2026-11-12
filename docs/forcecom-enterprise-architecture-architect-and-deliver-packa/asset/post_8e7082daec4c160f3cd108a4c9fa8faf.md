
That said, if you have use cases that result in a single database operation then you can of course consider using this feature, safely assured that your Apex Trigger and Domain layer code will continue to enforce your data validation and integrity.

Finally, note that querying records from JavaScript does not invoke your Selector code. So the fields queried and populated in the resulting SObject records on the client will not always be the same in all cases throughout your JavaScript code.

## Considerations for using JavaScript libraries

As discussed, the more you move towards a stateless controller and JavaScript Remoting client architecture, the more you need to invest in providing or obtaining client-side components not presently provided by Salesforce.

Consider this decision carefully on a per-case basis (not all of your application UI has to use the same approach) and make sure that you appreciate the value of the platform features that you are leaving behind. Expect to adjust your expectations around your client developer's velocity to be more in alignment with those on other platform developers.

Libraries such as jQuery provide a great deal of convenience, flexibility, and components available in the community to provide some alternatives and improvements over the Visualforce components, in addition to larger components such as grids and trees. Taking things a step further, larger commercially available libraries such as Sencha ExtJS are also usable from within a Visualforce page, including mobile variants such as jQuery Mobile and Sencha Touch.

> Lightning implements a security layer around your components, known as the Locker Service. This blocks eval, enforces use of strict mode, and restricts access to the DOM outside your components own boundaries. Check that the third-party libraries you intend to use in Lightning Components are compatible with this type of security context.

All of these libraries require a good deal of JavaScript programming knowledge, including its OOP style of programming. Some provide the Model View Controller (MVC) frameworks as well, such as AngularJS. If you search on Google for Salesforce and for example AngularJS, you will find many examples and articles from the community and the Salesforce Developer Evangelists team. Here are some aspects of developing with such libraries, which you should consider:

- **Developer flow**: What is the process from storing the client code in source control, pushing it into an org for development, editing code, debugging it, and pushing it back into Source Control? Likewise when it comes to packaging, consider whether you should compress your JavaScript or not. The Developer flow has to be fast; editing JavaScript files, rebuilding zip files, and uploading new static resources is not fast. Consider whether you can support a loose file development flow for client developers, such as using Visualforce components, page, or static resources for each file (**MavensMate** has good support for this), and then combine these into a single static resource for packaging and deployment.

- **Security**: Salesforce takes a great deal of care with their Visualforce and Lightning components to ensure that the escape values are displayed by the components to prevent attacks such as HTML injection for example. Check your components' escape values that they inject into the DOM from your database records, or you could find yourself open to attack and delays getting through the Salesforce Security Review.

- **Testing frameworks**: There are a number of approaches when testing rich clients, ranging from unit testing frameworks that have the developer write a JavaScript code to testing commercial applications that actually drive the web browser and assert the state of the page. Make sure that you invest according to the amount of JavaScript code your application contains.

## Custom Publisher Actions

In a previous chapter, we created a **Publisher Action** to mark a **Contestant** as a **DNF** (Did not Finish) object easily from a Chatter feed. This leveraged the declarative approach to creating Publisher Actions, based on creating or updating a related record. If your use case does not fit into these type of operations, you can use Lightning Component to develop a Custom Publisher Action.

## Creating websites and communities

Salesforce provides the following two ways to create public-facing web content:

- **Force.com**: This offers a means to create public-facing authenticated or public websites using Visualforce pages. Due to this, it can access Standard and Custom objects using the approaches described in this chapter, reusing components and services from your application as needed. The Force.com site configurations cannot be packaged, though the pages and controllers you create to support them can be. This feature is available in the Enterprise and Developer Edition orgs. It is possible to use Lightning Components via Lightning Out for Visualforce pages.

- **Sites.com**: This is a declarative website product that is targeted at nontechnical users. Visualforce pages cannot be used directly with Sites.com though components available with Sites.com can access your application's Custom Objects and as such will invoke your Apex Trigger and thus Domain logic code. It is possible to use Lightning Components with Sites.com via Community Builder.

## Mobile application strategy

Mobile application development using Salesforce has been a hot topic for the last few years, starting with the use of various well-known mobile frameworks such as jQuery Mobile, AngularJS along with Salesforce’s own APIs, including oAuth for authentication, and Salesforce REST API to access Standard and Custom Object records, leading up to the current development around the latest Salesforce1 mobile application.

The interesting thing about this is that it has evolved in a different way to the browser UI, which started with the standard declarative-driven UI and could then be augmented with a more developer-driven solution such as Visualforce. For a mobile UI, up until the release of Salesforce1, we only had the option of building something with a developer.

As things stand today, we now have both options available for building mobile UIs. As such, make sure that you understand first and foremost the capability of the Salesforce1 mobile application and its ability to surface your objects through its standard UI. Publisher Actions provide an excellent way to enhance the Chatter feed presence of your application and also add the same actions to your users' Salesforce1 experience.

Salesforce1's user interface model is not for everyone; it is, like the browser UI, mostly data-centric if you wish to expose a more fine-tailored process-driven user experience for a very specific mobile application. If this is something you feel you need, you can review Salesforce Mobile Packs to choose the architecture and APIs that suit your needs the best (https://developer.salesforce.com/mobile).

## Custom reporting and the Analytics API

Sometimes, the standard output from the Salesforce Reporting engine is just not what your users are looking for. They require formatting or a layout not supported by Salesforce, but the way in which they have defined the report is appealing to them.

The Salesforce Analytics API allows you to build a Visualforce page or mobile application that can execute a given tabular, summary, or matrix report and return its data into your client code to be rendered accordingly. The API is available directly to Apex developers and as a REST API for native mobile applications.

> You might want to consider using a report to drive an alternative approach to selecting records for an additional process in your application by leveraging the flexibility of the Report Designer as a kind of record selection UI.

## Updating the FormulaForce package

Apply the code included in this chapter into your package org. Apart from the RaceResultsController Apex class, all components provided with the sample code for this chapter will be included in the package automatically as they are additions to or referenced from components that are already packaged.
