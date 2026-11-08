# Understanding Angular routing

Often, you'll need to implement routing in your web applications because you can't fit all of your app's features on a single page — this creates a poor user experience and even makes some tasks impossible for users, such as, in our case, the ability to access individual profiles via their unique URLs or to make profiles findable by search engines via **Search Engine Optimization (SEO)**.

As an all-in-one platform, Angular has a built-in router module that simplifies the implementation of single-page apps with numerous views and navigation. Additionally, the Angular CLI may aid you by automatically configuring the routing module in your app during the project creation process.

Angular Router is a static routing library. In contrast to dynamic routing, the routes are specified at the application's initialization. You may implement routing in a single routing module that contains all routes for your application, or you can add routing to each module individually.

Following that, we'll explore how Angular Router works and how the Angular CLI automates the process of configuring routing in our project.

# How the Angular Router works

Instead of making requests to the server to obtain a new page, you can manage what users see in your Angular application by displaying or hiding the views controlled by components. This is accomplished by **client-side routing** rather than server-side routing.

To provide a great user experience, your application should present a distinct view for each activity or related group of activities, based on your business domain, rather than incorporating all functions into a single view. Additionally, it should enable navigation between distinct views — this may be accomplished through the use of Angular Router and browser URLs.

Angular Router is a client-side JavaScript solution that operates by monitoring changes to the URL and mapping paths to views using a route/component mapping array.

You indicate how paths are mapped to components in your routing module, and the router handles displaying the appropriate component when a path is matched (that would be when a user navigates to a certain path that is associated with a routing entry).

The router also allows you to provide a general wildcard entry that will be used when no matching paths are identified.