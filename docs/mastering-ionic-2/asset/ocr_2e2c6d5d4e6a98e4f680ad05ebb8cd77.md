# # Controllers match components

Components are the backbone of Angular and Ionic applications; you will see that almost everything is a component. You can compare controllers in Angular 1 to components in Angular 4. In controllers we used to define most of our code's logical part. We used to register the controllers with our main module. Now, in a similar way, we define our code in components in Angular 4 and if required we can export that

```
Component Class: // Angular & Ionic 1

angular.module('wedding.controllers', [])
.controller('LoginCtrl',
function($scope, CategoryService) {
// controller function and DI of CategoryService
}
);
```

```
// Angular 4 & Ionic 32

import {Component} from '@angular/core';
import {NavController} from 'ionic-angular';

@Component({
templateUrl: 'build/pages/catalog/categories.html'
})
export class CategoryPage {
// DI of NavController for navigation
constructor(private navCtrl: NavController) {
this.nav = navCtrl;
}
}
```

We have dependency injection in Angular 1 as controller function arguments, while in Angular 4 we pass it inside the constructor function. Many other things such as the IIFE syntax, which we have to define in Angular 1 for keeping out controller code of the global namespace, now are not required in Angular 4 because ES 2015 modules handle name spacing for us. Also, as you can see, we have exported the CategoryPage class, we can now import it wherever this module is required. Another major change is that $scope is replaced by the this keyword. $scope had many performance issues and already in Angular 1 developers have reduced the usage of $scope.