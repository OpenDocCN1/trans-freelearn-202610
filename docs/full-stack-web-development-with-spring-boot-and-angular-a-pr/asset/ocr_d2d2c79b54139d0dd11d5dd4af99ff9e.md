We have successfully configured and started our Angular application locally. Now, let’s discuss the concepts we will use to build our application.

# Angular features

The Angular framework is a component-based framework that allows us to develop reusable components to promote the reusability and maintainability of code. It offers many features that will make our frontend development more powerful. In this section, we will discuss the basic features and fundamentals of Angular that serve as its building blocks; take note that we will not discuss all of the features here, as we will focus more on the organization of the project.

To learn more about the features, you can visit the official documentation for Angular: https://angular.io/start.

# Components

Components are the main building blocks in Angular. They are responsible for defining what the behavior and the look of the view will be. Components are annotated with the @Component decorator, which assigns several types of metadata that describe the component’s HTML, CSS, and selector.

The following are the commands to generate a component:

```
ng generate component <component-name>
ng g c <component-name>
```

The following is a code example for a component:

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  title = 'superheroes';
}
```

Components have a feature called data binding, which allows us to pass data into the view. Data binding can be used to display values to the user, respond to user events, and modify the styles. Angular binding is classified into two groups:

+   - One-way binding
+   - Two-way binding