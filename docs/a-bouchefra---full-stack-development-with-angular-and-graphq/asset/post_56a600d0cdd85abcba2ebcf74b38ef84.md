
5. Go to the core/components/header/header.component.html file and change the *ngIf expressions from using the authService.isLoggedIn property, which used to return a Boolean value but now returns an Observable, to using the component's isLoggedIn property. For example, the search bar should be rendered as follows:

```
<div *ngIf="isLoggedIn">
  <input class="searchbar" type="text" placeholder="Search" #searchInput matInput>
  <button mat-icon-button (click)="searchUsers()">
    <mat-icon>search</mat-icon>
  </button>
</div>
```

You must do the same thing to conditionally render the buttons on the right.

Also, for the button that takes users to their profiles, replace the interpolated authService.authUser?.id expression between {{ and }} with just authUser?.id, as follows:

```
<button *ngIf="isLoggedIn" mat-icon-button
  matTooltip="My profile"
  routerLink="/users/profile/{{authUser?.id}}">
```

For all updates made to the header component, check out this commit: https://git.io/JVyW0.

If you check Apollo Client Devtools after you log in, you should see an active getAuthState query that will remain active as long as the header component is not destroyed, as illustrated in the following screenshot:

![](img/c798729e4c42765fcb1c98950164d0ba_236_0.png)

Figure 8.1 – getAuthState query in Apollo Client Devtools

## Using the OnPush change detection strategy

Angular employs the ChangeDetectionStrategy.Default strategy by default. This strategy is not the most efficient in terms of performance because every time an event occurs in our application, such as clicks or network responses, change detection runs on all components. To avoid unnecessary change detection runs, we can use the OnPush strategy for some components, as shown in the following steps:

-   1. Go to the core/components/header/header.component.ts file and import ChangeDetectionStrategy and ChangeDetectorRef from the @angular/core module, as follows:

```
import { Component, OnInit, ElementRef, ViewChild,
  OnDestroy, ChangeDetectionStrategy,
  ChangeDetectorRef } from '@angular/core';
```

-   2. Change the detection strategy to OnPush, like this:

```
@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class HeaderComponent implements OnInit,
  OnDestroy {
  /* [...] */
}
```

Afterward, you will notice that the search bar and the buttons on the right are not properly rendered if you log out and log back in. This occurs because Angular does not run change detection to synchronize the view with the component's updated variables—namely, the authUser and isLoggedIn members. Let's fix that by manually triggering change detection whenever the auth state changes.

-   3. Inject ChangeDetectorRef into the header component's constructor, like this:

```
constructor(
  /*...*/,
  private changeDetectorRef: ChangeDetectorRef) { }
```

4. Mark the component for checking in the header component's ngOnInit() method, as follows:

```
ngOnInit(): void {
    this.authService.authState
    .pipe(takeUntil(this.destroyNotifier$))
    .subscribe({
        next: (authState: AuthState) => {
            this.isLoggedIn = authState.isLoggedIn;
            this.authUser = authState.currentUser;
            this.changeDetectorRef.markForCheck();
        }
    });
}
```

Use the OnPush detection strategy for the footer and page-not-found components as an assignment. The updates and solutions can be found at this commit: https://git.io/JV9zb.

## Testing the auth service and component(s)

Before we finish implementing the auth section, we need to add automatic tests to our implementation to detect any hidden errors that we might miss when manually testing.

When we generate our project with the Angular command-line interface (Angular CLI), we also have testing configured with a number of initial tests that are added to ensure that artifacts, such as components and services, are properly instantiated.

If you're new to Angular testing, start by reading https://angular.io/guide/testing and https://techiediaries.com/angular/jasmine-unit-testing, then learn how to write your first test suite at https://jasmine.github.io/tutorials/your_first_suite.

Also, before testing Apollo Client APIs, make sure to read the official testing documentation at https://apollo-angular.com/docs/development-and-testing/testing/.

Jasmine includes a set of APIs that make it simple to write unit tests. We basically have three options, as follows:

-   - The describe() function is used to define a set of tests.
-   - The it() function is used to initialize a unit test/specification.
-   - The `expect()` function is used to define expectations to ensure that our code works as expected.

When we generate an artifact, such as a component or service, with the Angular CLI, we also get a specification file that includes tests for the artifact, as well as a unit test that checks whether the artifact is instantiated.

### Testing the auth service

Your project is ready for testing. All you have to do is follow the upcoming steps in this section.

Go to your Terminal and type the following command:

```
ng test
```

The ng test command builds the application and runs the Karma test runner in watch mode. Visit https://github.com/karma-runner/karma for more information. This command will run our tests and open a web browser with either passed or failed tests. At this point, we would expect all tests to pass, but this is not the case. This is due to the fact that the tests, not the code, are broken.

This is because we modified the default project's code generated by the Angular CLI by injecting some services via the constructor or adding code to the components' ngOnInit() method. Because they are involved in the creation of components, the constructor and ngOnInit() methods are both called in the existing unit tests.

Let's start by testing the auth service before we fix the broken tests. If you are unfamiliar with testing services, read https://angular.io/guide/testing-services to better understand these steps.

Most of our service's methods follow a similar pattern—we request data, handle errors, and extract data from the response, so they should be tested in much the same way.

We must mock/fake the response and then assert that queries and mutations are called once or multiple times with the expected documents and variables, and that the mocked response data is returned. Proceed as follows:

-   1. Return to your Terminal and run the tests for the auth service only, using the following command from the client/ folder:

```
ng test --include src/app/core/services/auth/auth.service.spec.ts
```

As you can see, the existing *should-be-created* test case passes; let's write some more. Open the core/services/auth/auth.service.spec.ts file and add the following specs:

```
it('should register a user');
it('should reset auth state when registration fails on server');
it('should authenticate a user');
it('should reset auth state when login fails on server');
it('should store user and token on updateAuthState call');
it('should call localStorage.setItem on storeUser call');
it('should call localStorage.setItem on storeToken call');
it('should return the isLoggedIn state');
it('should return the authenticated user state');
it('should return the full auth state');
it('should log out users');
it('should search for users');
it('should get user by ID');
```

If you go to your Karma report before implementing the actual test logic, in the second parameter of the it() function, you'll see that your specs are marked as pending, as illustrated in the following screenshot:

![](img/c798729e4c42765fcb1c98950164d0ba_240_0.png)

Figure 8.2 – Pending tests of the auth service

We'll use the apollo-angular/testing module, which includes ApolloTestingModule and ApolloTestingController, to make it simple to test code that calls Apollo Client APIs by mocking calls to the GraphQL endpoint.

2. Begin by including the following imports:

```
import {
    ApolloTestingController,
    ApolloTestingModule
} from 'apollo-angular/testing';
import {
    AuthResponse,
    RegisterResponse,
    REGISTER_MUTATION,
    User
} from 'src/app/shared';
```

3. Let's now extend the describe() block by defining a testing controller, as follows:

```
let controller: ApolloTestingController;
```

Then, create a fake user object, as follows:

```
const fakeUser: User = {
    id: 'id#1',
    fullName: 'A B',
    username: 'a.b',
    email: 'a.b@email.com'
} as User;
```

Create a fake token, as follows:

```
const fakeToken: string = '<ADD_A_TOKEN_HERE>';
```

You can get a valid token from your server using Apollo Studio, or use an online service such as https://token.dev/.

4. Then, using the fake user and token, create an auth response, as follows:

```
const authResponse: AuthResponse = {
    token: fakeToken,
    user: fakeUser
};
```
