
If a component matches, the router will display it in router outlet. router outlet is an Angular directive that serves as a place for the component(s) that match the current URL.

> **Important Note**
> In Angular, a directive is a concept that allows you to expand HTML with custom tags that are only available in the context of your application. Many built-in directives, such as NgFor and NgIf, are provided by Angular, and you may create your own by using the @Directive decorator.

The router may be found in the @angular/router module. By importing this module, you will have access to all of the services, components, and directives required to build routing and navigation in your Angular project.

Returning to the section *Initializing the Angular project*, when you created your project, the Angular CLI asked if you wanted to add routing. By choosing yes, you told the CLI to automatically set up routing, resulting in the generation of the routing module within the src/app/app-routing.module.ts file. You also instructed the Angular CLI to generate a routing module for the users and feed modules, to provide routing for each module's components in the *Creating modules, services, and components* section using the --routing option.

To implement routing, all you need to do now is add the route/component mappings inside the routing modules, but first let's look at what the CLI has done to set up routing.

## The <base href> tag

When you view the src/index.html file, you will notice that a <base> tag has been inserted as a child of the <head> tag. This assists the router in constructing navigation paths. This is an example of your Angular application's index.html file after you've added routing:

```
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Client</title>
    <base href="/">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

The <base href> tag is an HTML tag that lets you define the base URL that will be used to determine the absolute URL for any relative URLs on your document.

## The routing modules

The CLI has added a routing module to the src/app/app-routing.module.ts file for setting routing, as seen below:

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

This code imports the Routes and RouterModule symbols from the @angular/router package that provides the necessary router APIs. Next, it defines a routes array of the Routes type. This array will be used to add any routes in our application.

Next, RouterModule is added via the imports array of the routing module, and its forRoot() method is called with the previously defined routes array. RouterModule is then exported from the routing module using the exports array.

AppRoutingModule is simply an Angular module that wraps an instance of RouterModule with the routes array that will be used to hold any routes in our application. RouterModule is a built-in module that provides the service and directives required to implement routing, such as RouterLink, RouterLinkActive, and RouterOutlet.

The same routing module was added to the users and feed modules.

## The router outlet

A router-outlet directive is added to the root application component in addition to the routing module. Go to the src/app/app.component.html template and look for some HTML code containing a <router-outlet> custom tag, as seen here:

```
<!-- [...] -->
<router-outlet></router-outlet>
```

The router outlet is a directive imported with the other routing APIs from the @angular/router module, and it serves as a marker to tell the router where to insert and render the component(s) matching the browser's URL, based on the information passed to the routes array described earlier.

## Importing the routing module

The CLI also imported AppRoutingModule in the root application module, which can be found in the src/app/app.module.ts file, using the following code:

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HeaderComponent } from './core/components/header/header.component';
import { FooterComponent } from './core/components/footer/footer.component';
import { PageNotFoundComponent } from './core/components/page-not-found/page-not-found.component';

@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    FooterComponent,
    PageNotFoundComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

AppRoutingModule is imported from the app-routing.module.ts file and added to AppModule imports array. The same was done in the users and feed modules for each routing module.

We've documented all of the steps that the Angular CLI took automatically to establish routing in our project. In the following part, we'll look at how to add routes to our previously created components in order to provide in-app navigation.

## Understanding and adding routes

As previously stated, the Angular CLI automatically configured routing when creating our project, so all we need to do now is specify the routes to the components of our application that we previously created.

Not all components are routable – in other words, they should not be mapped to particular paths. For example, the header component will be included in other components to render the header element of the page through its selector, therefore we do not need to include it in our routing module's routes array.

A route is a Route object that holds information about which component maps to which path. A path is a URL element that specifies which view should be navigated to.

A route may have one or more of the following attributes:

- **path**: A property that holds the route's path.
- **pathMatch**: A property for configuring the matching strategy that the router will use to match the path. It can have a `prefix` or a `full` value, with the `prefix` being the default.
- **component**: A property referring to the component that should be mapped to the route.
- **redirectTo**: A property that contains another path to which you wish to be redirected if the path in the `path` property matches.
- **loadChildren**: A property to lazy-load a module by the router.

These are the most commonly used features of a route, but we may also utilize other properties, which can be found in the official documentation.

Consider the following example: we want the router to render the posts component if we browse to the /posts URL. All we have to do is add the following route configuration to the routes array:

```
{ path: 'posts', component: PostsComponent }
```

We may also have special paths, such as the empty path, which refers to the root URL of your application's domain name, or a wildcard path marked with **, which refers to a default route that the router will match if the user navigates to a path that does not exist in the routes array. You may use it to display a 404 error page informing the user that the page does not exist, or you can simply redirect the visitor to another URL.

Following that, we'll learn about the router's matching strategies for matching routes.

## Route-matching strategies

The matching algorithm in the Angular router provides two strategies: prefix and full, however you may also use custom matching strategies:

- The router checks if the start of the browser's URL is prefixed with the route's path and renders the appropriate component using the default prefix strategy.
- The router uses the full strategy to determine if the path segment of the browser's URL is identical to the path of the route.

If you need to match the entire path, just set the pathMatch attribute of the route to full strategy, as seen here:

```
{ path: 'posts', pathMatch: 'full', component: PostsComponent }
```

The scenario of an empty path is another typical example of matching the full path, because the default prefix strategy will match all paths since the empty string is part of any path. For example, we can use the following route configuration to redirect visitors to the /home path when they first visit our application:

```
{ path: '', redirectTo: '/home', pathMatch: 'full' }
```

We employ redirectTo instead of the component attribute, which accepts the path to which users will be routed.
