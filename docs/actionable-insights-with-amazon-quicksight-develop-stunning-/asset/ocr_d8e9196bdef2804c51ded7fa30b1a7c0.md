After adding these components, our architecture diagram will look as follows:

![](bbox=[0.117, 0.13, 0.848, 0.714])

Figure 7.5 – Dashboard embedding with AWS services

The flow for dashboard embedding would be as follows:

+   1. The user uses their web browser to open the web portal application.
+   
+   2. The user authenticates with the identity provider and accesses the resource provider via the web application.
+   
+   3. The web application assumes the embedding IAM role.