We can see in the preceding example that our login is successful and the /authenticate endpoint returned a valid token. We can now use the token in the request header to send a request to anti-hero endpoints:

![](bbox=[0.14, 0.17, 0.85, 0.52])

Figure 7.5 – Anti-hero endpoint returns the list successfully

We can see in the preceding example that we have used the generated token in our authorization header, and we have received a 200 response and returned the list of anti-heroes.

We have now successfully created the custom authentication and authorization for our Spring Boot application using Spring Security. In the next section, we will discuss an additional topic relating to security, called IDaaS.

# IDaaS

In the previous section, we created our custom login authentication using Spring Security. We utilized some of the features of Spring Security and also used JWT to store user states and validate credentials. However, this example is not enough of a reliable and secure way of implementing authentication for our application.

Large and enterprise applications nowadays demand several security features to be able to prevent possible vulnerabilities that can occur. These features can include the architecture and the implementation of