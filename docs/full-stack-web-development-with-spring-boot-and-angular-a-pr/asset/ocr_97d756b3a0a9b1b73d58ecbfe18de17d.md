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

# The authentication module

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