That said, if you have use cases that result in a single database operation then you can of course consider using this feature, safely assured that your Apex Trigger and Domain layer code will continue to enforce your data validation and integrity.

Finally, note that querying records from JavaScript does not invoke your Selector code. So the fields queried and populated in the resulting SObject records on the client will not always be the same in all cases throughout your JavaScript code.

# Considerations for using JavaScript libraries

As discussed, the more you move towards a stateless controller and JavaScript Remoting client architecture, the more you need to invest in providing or obtaining client-side components not presently provided by Salesforce.

Consider this decision carefully on a per-case basis (not all of your application UI has to use the same approach) and make sure that you appreciate the value of the platform features that you are leaving behind. Expect to adjust your expectations around your client developer's velocity to be more in alignment with those on other platform developers.

Libraries such as JQuery provide a great deal of convenience, flexibility, and components available in the community to provide some alternatives and improvements over the Visualforce components, in addition to larger components such as grids and trees. Taking things a step further, larger commercially available libraries such as Sencha ExtJS are also usable from within a Visualforce page, including mobile variants such as JQuery Mobile and Sencha Touch.

> Lightning implements a security layer around your components, known as the Locker Service. This blocks eval, enforces use of strict mode, and restricts access to the DOM outside your components own boundaries. Check that the third-party libraries you intend to use in Lightning Components are compatible with this type of security context.

All of these libraries require a good deal of JavaScript programming knowledge, including its OOP style of programming. Some provide the Model View Controller (MVC) frameworks as well, such as AngularJS. If you search on Google for Salesforce and for example AngularJS, you will find many examples and articles from the community and the Salesforce Developer Evangelists team.