
## Modules

**Modules** are one of the essential features of the Angular framework. As our application gets more complex, it will consist of large blocks of components, directives, and services. This will affect the maintainability of the application’s code base. The Angular framework provides a way of organizing and grouping blocks, known as modules.

Modules in the Angular framework help us develop our application promoting the separation of concerns. They allow us to classify and organize blocks based on their functionality. Angular is also built using modules; the `@angular/core` framework is the primary Angular module, which provides Angular’s core functionalities and services.

### Creating a module

We will use the `@NgModule` decorator to create a module. It consists of several types of metadata that allow us to define the component, services, pipes, and other modules included in the created module.

The following example code shows the properties available for a module:

```
@NgModule({
  declarations: [],
  imports: [],
  providers: [],
  exports: [],
  bootstrap: [],
  entrycomponents: []
})
```

Let’s now discuss the functionality of each property:

- **Declarations**: This is where we place the components, directives, and pipes of our applications. Remember that components, directives, and pipes must be declared in only one module.
- **Providers**: This is where we place services to allow them to be available for dependency injection.
- **Imports**: This is where we place one or more other modules in our application. Once we import a specific module, all components, pipes, and directives in the imported module can be used.
- **Exports**: This is where we place the components, directives, and pipes to be available to other modules once imported.
- **Bootstrap**: This is where we place the main component of the module. The Bootstrap property is used mainly for the root module of the Angular application (`AppModule`), as the root module’s responsibility is to load the first view as our application starts.

- **Entry components**: This is where we place components that should be dynamically loaded in our application.

The following diagram shows how NgModule works in an Angular application:

![](img/7c3ddd2e5893c9b5a0220148ed886548_239_0.png)

Figure 10.5 – Diagram for the flow of an Angular module

### Services and dependency injection

**Services** are also one of the valuable features of Angular. It is code that can be reused in different components of your application.

The primary responsibility of services is the following:

- Reusing logic on different components
- Implementing API communication and data access
- Promoting single responsibility, as it separates the independent features of components

To create a service in the application, we will create a class and annotate it with the @Injectable decorator. To register the service at the root level of your application, we will add the following to our @Injectable decorator:

```
@Injectable({
  providedIn: 'root',
})
```

Once we have set the value of the providedIn metadata in the root, this will create a single, shared instance of the service throughout the application. On the other hand, if we want to provide the service in a specific module, we will place the service in the provider metadata of @NgModule:

```
@NgModule({
  providers: [Service1]
})
```

Now that we have discussed some of the essential features of Angular, we will focus on how to structure your Angular application.

### Creating the Angular folder structure

The Angular framework is considered an MVW framework, which means that there are many possible ways of structuring our application. In this case, we would discuss one of the best practices or most commonly used structures that can help your Angular application be scalable and maintainable.

In the previous part, we learned that Angular blocks could be grouped and organized into modules; modules are a good starting point for structuring our Angular application. The first step we can implement is to group and classify modules based on their functionality. Listed here is the classification of our modules.

### Root module

The **root** module is the application’s main module. This is the first module loaded as the Angular application starts. The root module is automatically generated when we scaffold an Angular application called `AppModule` and is found under the `src/app` folder.

### Feature module

The **feature** module is where we place modules that apply a specific feature of our application. This means that most of what our code uses is inside this module. We will create the components, pipes, and directives under the module where they should be included and we can also separate components by placing the components that have a route in a page folder.

Let’s have a look at an example folder structure of a feature module called `AntiHeroModule`:

```
src
├─ app
│  ├─ anti-hero
│  │  ├─ components
│  │  │  ├─ shared.component.ts
│  │  ├─ directives
│  │  │  ├─ first.directive.ts
│  │  │  ├─ another.directive.ts
│  │  ├─ pages
│  │  │  ├─ form
│  │  │  │  ├─ form.component.ts
│  │  │  ├─ list
│  │  │  │  ├─ list.component.ts
│  │  │  ├─ anti-hero.component.ts
│  │  │  ├─ anti-hero.component.html
│  │  │  ├─ anti-hero.component.css
│  │  │  ├─ index.ts
│  │  ├─ pipes
│  │  │  ├─ first.pipe.ts
│  │  ├─ anti-hero.module.ts
│  │  ├─ anti-hero.routing.module.ts
│  │  ├─ index.ts
```

In the folder structure here, we have divided our anti-hero module into several parts. The first folder is the components folder, which contains all of the components shared throughout this module. These can also be called dumb components, which we will discuss in the next section.

The next two are the directives and pipes folders, which contain the directives and pipes used in AntiHeroModule. Lastly, the pages folder includes the components in AntiHeroModule that have a direct route. These can also be called smart components. We have also included the index.ts file, known as a barrel file, which will provide a centralized place for exporting components, directives, and pipes.

### Shared module

The shared module is a module that is used and shared throughout the application; this is composed of the components, pipes, and directives that we are required to use in different parts of the application. Remember that a shared module should not have a dependency on other modules in the application.

Shared modules are created under the src/app/shared folder.

Let's have a look at the example folder structure of a shared module in our application:

```
|— src
| |— app
| | |— shared
| | | |— layout
| | | | |— footer
| | | | | |— footer.component.ts
| | | | | |— footer.component.html
| | | | |— header
| | | | | |— header.component.ts
| | | | | |— header.component.html
| | | | |— layout.module.ts
| | | |— index.ts
```

### Blocks under the features module

We want to create the blocks under our features module; the first thing we need to scaffold is AntiHeroModule. Execute the ng g m anti-hero command to generate the module under the src/app folder.

Now, under the anti-hero folder, we will create the following folders:

- components: This will contain components that will be shared in this module.
- pipes: This will contain all the pipes used by the anti-hero module.
- directives: This will contain all the directives that the anti-hero module will use.
- pages: This will contain components that have a direct route.

After creating the folders, we will now make the page components for our anti-hero module. We will add two pages where the first one will be the page for displaying the list of anti-heroes and the second one will be a form that allows us to view, create, or modify the selected hero in the list. We can execute the ng g c anti-hero/pages/pages/form and ng g c anti-hero/pages/list commands to make the two pages. This will create two new components, form and list, under the pages folder.

After successfully creating the page components, we will also add a routing module for our anti-hero module. We will execute the ng g m anti-hero/anti-hero-routing --flat command and we will place the following code in our routing module:

```
import {NgModule} from "@angular/core";
import {RouterModule, Routes} from "@angular/router";
import {ListComponent} from "./pages/list/list.component";
import {FormComponent} from "./pages/form/form.component";

const routes: Routes = [
  {
    path: "",
    component: ListComponent,
  },
  {
    path: "form",
    component: FormComponent,
  },
];
@NgModule({
```
