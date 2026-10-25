
The embedded dashboard is in the form of a URL with an authorization code. To get the embedded dashboard URL, you will need to call the GetDashboardEmbedUrl API call. This API call returns a session URL with an authorization code that can be used to embed the dashboard into your website. The GetDashboardEmbedUrl API call needs to be initiated by the web application server, not from the user's browser. To add an embedded dashboard URL to your website, you can use the embeddedDashboard(options) method from the QuickSight Embedding SDK. This allows you to easily set a parameter value so that your dashboard starts with specific initial parameters. Other options include the embedded frame formatting, allow/not allow printing, scrolling behavior, and starting sheet. For more information, you can read the QuickSight Embedding SDK documentation: https://github.com/aws labs/amazon-QuickSight-embedding-sdk.

## Understanding QuickSight console embedding

You can embed the full QuickSight console application into your web application. The embedded console capabilities are only available to authenticated QuickSight users. As expected, anonymous access is not an option for this type of embedding. Similar to the QuickSight web app, only authenticated users can access the QuickSight console. Console embedding will provide your users with the full QuickSight experience. You will be able to have both reader and author users using it. Authors will be able to configure datasets, perform analysis, and share dashboards, while your readers will be able to access read-only dashboards. The console session is in the form of a URL with an authorization code. To get the console embedding URL, you will need to use the QuickSight GetSessionEmbedUrl API action. Once you have the URL, you can use embedSession(options) from the QuickSight Embedding SDK to embed the QuickSight console into your web application. For more information, go to https://github.com/aws labs/amazon-QuickSight-embedding-sdk.

Now that we've discussed the two types of embedded analytics, in the next section, we will discuss the embedded analytics architecture.

## Exploring the architecture and user authentication

In this section, we will focus on the architectural components of embedded analytics. To understand this end-to-end architecture, we will break it down into three layers:

- Web application layer
- BI and data layer
- Authentication and authorization layer

This is better represented with the following diagram:

![](img/33bad1d211a2d93fd743a1978d403f9a_153_0.png)

Figure 7.1 – Generic architecture for embedded analytics

Generally, a client typically accesses a web app or a web portal using their web browser. In many cases, the user will need to authenticate with the web app. The client will present user credentials (typically, a username and a password) to the **authentication layer**, which sends back an access code/token so that the client can communicate with the **web application layer**. In embedded analytics, the web application layer will be responsible for getting the embedded visuals from the **BI layer**, which, in turn, would be responsible for querying the data from the data sources and visualizing them (**data layer**).

In the next section, we will analyze each of these architectural components and identify AWS services that can be used to build them.

## Overview of the web application layer

In embedded dashboarding applications, embedded dashboards are embedded within this layer. The web application layer interfaces with both the authentication layer and the dashboarding layer. A web application is typically accessed by the user’s web browser. The web application layer is responsible for sending the application’s content back to the browser user. Ensuring that the web application layer has enough resources to support your users is your responsibility, and you will need to make sure that there is enough capacity for your web server to accept incoming connections. For that reason, you can consider AWS serverless services such as **Amazon Lambda**, **Amazon S3**, for static content, and **Amazon API Gateway** to build a serverless web application that scales automatically to the incoming demand. Other options include the other compute and container services, such as **Amazon EKS**, **ECS**, **Fargate**, and **EC2**. You can also host this layer outside of AWS, which means that you don’t need to move your application to the AWS cloud to make use of QuickSight’s embedded analytics. For embedded analytics to work, you will need to whitelist the domain of your web application in QuickSight’s settings.

To whitelist a domain in Amazon QuickSight, follow these steps:

1. Log in as an admin user and select **Manage QuickSight** from the top right-hand corner’s drop-down menu.

2. Next, select **Domains and Embedding** to go to the menu shown in the following screenshot:

Edition: Enterprise

![](img/33bad1d211a2d93fd743a1978d403f9a_154_0.png)

Figure 7.2 – Domains and embedding settings

3. Type in the domain of your web application and click **Add**. Note that you have the option to include subdomains if you need to by simply checking the **Include subdomains** checkbox.

Now that we’ve discussed the web application layer, we will move on to the BI layer.

## Overview of the BI layer

The dashboard layer is the component that serves embedded dashboards to the web application layer. Amazon QuickSight is the central component of this layer. This layer can be embedded too, with the full console embedding. The BI layer is responsible for the following:

- Integrating with data sources
- Enforcing access to dashboards
- Serving embedded dashboard URLs

Concerning scalability, QuickSight will scale automatically for the number of users you configure. If you have anonymous embedding, you will need to enable session capacity planning. This layer interfaces with the data stores, eliminating the need to embed code that accesses the data stores into your application, which simplifies the integration with data sources. To work with datasets, you must use the QuickSight capabilities we learned about in Chapter 3, Preparing Data with Amazon QuickSight, rather than developing custom code. Visuals and insights are developed using QuickSight's native capabilities. Finally, at this layer, we configure which users have access to which dashboards. Authorized users get a dashboard URL with an authorization code, while users who don't have access won't get the dashboard URL. Next, we will talk about the authentication layer of an embedded analytics solution within QuickSight.

## Understanding the authentication layer

User authorization and authentication are important considerations when building embedded BI applications with QuickSight. The key questions to be addressed are as follows:

- How do you authenticate your users? (Authentication)
- Which users have access to which dashboards? (Authorization)

In embedded analytics, it is common that users authenticate with credentials that are stored outside of Amazon QuickSight in an identity store. You can use **Amazon Cognito** User Pool as an identity store for your cloud-native embedded analytics architecture. You can configure your identity provider with IAM so that you can assume IAM roles for your authenticated entities.

To configure an identity provider with IAM, follow these steps:

1. Log in as an AWS administrator and open the IAM console.
2. Click on the **Identity providers** option, as shown here:

![](img/33bad1d211a2d93fd743a1978d403f9a_156_0.png)

Figure 7.3 – Configuring identity providers

3. Select the type of identity provider (SAML or OIDC) and provide the required metadata for your identity provider.

Once you have set up the identity provider, you can configure trust relationships for IAM roles. Your web application will need to be able to assume an IAM role that has access to the required QuickSight APIs so that it can retrieve the embedding URLs.

An example policy that allows the caller to call the `GetDashboardEmbedUrl` and `GetAuthCode` functions for all resources is shown here:

```
{
 "Version": "2012-10-17",
 "Statement": [
```
