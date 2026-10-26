```
useFactory: createTranslateLoader,
deps: [Http]
}),
CloudModule.forRoot(cloudSettings)
],
bootstrap: [IonicApp],
entryComponents: [ ... ],
providers: [ ... ]
})
```

We have successfully now uploaded our vPlanet-Commerce application to ionic.io. We are now ready to start using Ionic services such as Push, Deploy, DB, and so on. You can find all your applications on the Dashboard (https://apps.ionic.io/apps):

![](bbox=[0.16, 0.22, 0.84, 0.375])

You can find all the information about the application inside the Settings tabs. After you click on your specific project, you will be able to see details about specific projects. Currently, we are using a free plan for our vPlanet-Commerce application, which gives around 5000 deploys, 10,000 push notifications, and 100 package builds. You can upgrade the plan at anytime as you will see that your application users are increasing and traffic is growing. What's best is that we will be integrating each and every Ionic Cloud service starting from Auth for user login and sign-up process. Ionic DB as our application database, Push Notification, Package, and Deploy:

![](bbox=[0.15, 0.57, 0.85, 0.828])

Also, another important point to note is that when you have completed setting up the Ionic Cloud, you will find app_id and api_key inside .io-config.json and some other app details inside ionic.config.json:

```
// .io-config.json
{"app_id":"f8fec798", "api_key":"5b22exxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx68e"}

// ionic.config.json
{
```