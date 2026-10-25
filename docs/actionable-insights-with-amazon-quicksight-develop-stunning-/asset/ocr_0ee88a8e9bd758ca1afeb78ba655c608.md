# # Overview of the web application layer

In embedded dashboarding applications, embedded dashboards are embedded within this layer. The web application layer interfaces with both the authentication layer and the dashboarding layer. A web application is typically accessed by the user’s web browser. The web application layer is responsible for sending the application’s content back to the browser user. Ensuring that the web application layer has enough resources to support your users is your responsibility, and you will need to make sure that there is enough capacity for your web server to accept incoming connections. For that reason, you can consider AWS serverless services such as **Amazon Lambda**, **Amazon S3**, for static content, and **Amazon API Gateway** to build a serverless web application that scales automatically to the incoming demand. Other options include the other compute and container services, such as **Amazon EKS**, **ECS**, **Fargate**, and **EC2**. You can also host this layer outside of AWS, which means that you don’t need to move your application to the AWS cloud to make use of QuickSight’s embedded analytics. For embedded analytics to work, you will need to whitelist the domain of your web application in QuickSight’s settings.

To whitelist a domain in Amazon QuickSight, follow these steps:

+   1. Log in as an admin user and select **Manage QuickSight** from the top right-hand corner’s drop-down menu.

+   2. Next, select **Domains and Embedding** to go to the menu shown in the following screenshot:

Edition: Enterprise

![](bbox=[0.133, 0.571, 0.866, 0.865])

Figure 7.2 – Domains and embedding settings