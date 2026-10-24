
## Summary

With this, we have reached the end of this chapter. Let’s have a recap of the valuable things we have learned; we have completed the CRUD features of applications using the building blocks of NgRx, and we have learned the difference between using and not using side effects in state management. Side effects are essential for our changes in the store to be synced with the database.

We have also learned, step by step, how to create the building blocks of NgRx with the different actions we need for our application.

In the next chapter, we will learn how to apply security features in Angular, such as adding user login and logout, retrieving user profile information, protecting application routes, and calling an API with protected endpoints.

## Adding Authentication in Angular

In the previous chapter, we completed the CRUD features of our Angular application using the building blocks of NgRx. We also learned the step-by-step process of writing the actions, reducers, and effects in our application that will be used to modify the value of states. We also learned the difference between using and not using effects in the application. Effects are essential for us to communicate with the external APIs that allow the database changes to be synced in the NgRx store.

In this chapter, we will learn how to add authentication in our Angular application; we will implement a login page that will provide a valid JWT, protect routes, and apply API authentication with the use of NgRx.

In this chapter, we will cover the following topics:
- Adding user authentication
- Protecting routes
- Calling an API

### Technical requirements

The complete code for this chapter can be found at: https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-14.

### Adding user authentication

Adding user authentication is one of the main requirements in developing an application. This feature allows us to restrict pages and features from unauthorized users. We can achieve user authentication in different ways, and one way to implement this is by providing a login page that will ask for credentials.

Let’s have a look at the step-by-step process of implementing the authentication feature.

## The authentication API

Let us first recap the authentication API we created in our Spring Boot project. The endpoints for authentication are as follows:
- `{BASE_URL}/authenticate`: The main endpoint for authentication accepts an object with email and password fields and returns a valid JWT that will be used for calling endpoints. The following is an example response object of the endpoint:

```
// valid JWT
{
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJOZXN0QGDtYW1sLmNvbSIImIhdCI6MTY1OTQyODk2MSwiZXhwIjIoxNjUSNDY0OTYxfQ.WU_aZjmI fw--LCovx4cZ4_hcOTGiAgPnSaM0bjdv018"
}
```
- `{BASE_URL}/register`: The endpoint for creating new valid credentials for login. JWT, as stated in Chapter 7, Adding Spring Boot Security with JWT, is used chiefly on RESTful web services that cannot maintain a client state since JWT holds some information connected to the user. This will be used primarily in the headers of endpoints that we will request.

In our project, let’s create a service named AuthenticateService under the core/services folder by executing the following command:

```
ng g core/services/authenticate
```

After successfully creating the service, we will place the following code in the service:

```
export class AuthenticateService {

  constructor(private http: HttpClient) { }
  // for login endpoint
  login(data: {email: string, password: string}): Observable<any> {
    return this.http.post<any>(
      `${environment.authURL}/authenticate`,
      data).pipe(
      tap((data: any) => data),
      catchError(err => throwError(() => err))
    )
  }
  // for register endpoint
  register(data: {email: string, password: string}):
```

```
Observable<any> {
  return this.http.post<any>(
    `${environment.authURL}/register`, data).pipe(
    tap((data: any) => data),
    catchError(err => throwError(() => err))
  )
}
```

AuthenticateService will hold the two endpoints we will use for our login page. Now, let’s create the interceptor for our application.

## The HTTP interceptor

HTTP interceptors are features of Angular that allow us to intercept HTTP requests to transform their headers, the body, or the response. This provides the intercept() function, which will enable us to get the outgoing request and call the next interceptor or the backend.

We will mainly use the interceptor to modify the headers of our endpoint requests, which will be responsible for adding the Authorization: Bearer {JWT} header for each invoked request.

To implement the interceptor, we will create the core/interceptors/header.interceptor.ts file, and we will place the following code within it:

```
@Injectable()
export class HeaderInterceptor implements HttpInterceptor {
  intercept(httpRequest: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const Authorization = localStorage.getItem('token') ? `Bearer ${localStorage.getItem('token')}` : '';
    if (httpRequest.url.includes('/api/v1'))
      return next.handle(httpRequest.clone({ setHeaders: {
        Authorization }
      }));
    else
      return next.handle(httpRequest);
  }
}
```

In the preceding code example, we have added a new implementation for the intercept() function. The first step is to retrieve the valid JWT in our local storage that will be used in the HTTP

headers. We will only use the JWT if the request endpoint has an api/v1 substring, as these are the endpoints that are protected.

The next step is to clone the request and add the Authorization: Bearer {JWT} header in the cloned request and call the next() function to call the API with the added header.

We have now created our interceptor; the last step is to add the interceptor in AppModule.

Let's have a look at the following code:

```
providers: [
  { provide: HTTP_INTERCEPTORS, useClass:
      HeaderInterceptor, multi: true }
],
```

In the preceding code example, we will now intercept every HTTP call on the anti-heroes endpoint and will add the generated JWT in the request headers.

## The authentication module

The next step is to create an authentication module; this module will be responsible for holding the login and registration page that will accept the users and credentials and call the authenticate and register endpoints.

To create the authentication module, we will execute the following command:

```
ng g m auth
```

After successfully creating the authentication module, we will import several modules we need for our authentication module:

```
@NgModule({
  declarations: [
  ],
  imports: [
    CommonModule,
    MaterialModule,
    FormsModule,
    ReactiveFormsModule,
    CoreModule,
  ]
});
```

Now, we will create the different parts of our module.

## The authentication form

We will create the main form for our authentication module; this is considered the dumb component of our module as it will accept and emit the values of the form to the login and registration page.

To create the authentication form component, we will execute the following command:

```
ng g c auth/components/auth-form
```

After successfully creating the component, we will now implement the form's code. In the TypeScript file of the auth-form component, we will place the following code:

```
export class AuthFormComponent {
  @Input() error: string = "";
  @Input() title: string = "Login"
  @Output() submitEmitter = new EventEmitter();
  form: FormGroup;
  constructor(private fb: FormBuilder) {
    this.form = this.fb.group({
      email: [''],
      password: ['']
    })
  }
  submit() {
    this.submitEmitter.emit(this.form.value);
  }
}
```

In the preceding code example, we can see that we have created a reactive form with an email and password form control. We have also created an emitter that will pass the values of the form into the parent component, as this component will be used by both the login and the register page. Now, we will implement the HTML code and the CSS of the auth-form component.

> **Note**
> Please refer to the link provided for the entire code implementation:
> https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-14/superheroes/src/app/auth/components/auth-form
