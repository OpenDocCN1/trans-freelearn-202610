# Configuring JDBC in WebSphere Application Server

Specify the connection pool properties. Connection timeout is the maximum number of seconds an application waits for a connection from the connection pool before timing out. Maximum connections is the maximum number of connections in the connection pool. Minimum connections is the minimum number of connections in the connection pool.

![](bbox=[0.178, 0.237, 0.822, 0.558])

# Deploying a Web Application to the WebSphere from JDeveloper

We will develop a JSP application to retrieve data from the DB2 database, and display the data in a browser. JDeveloper does not have the provision to connect to WebSphere application server, or deploy an application to WebSphere. JDeveloper can be used to create a JSP that can be deployed to WebSphere. Create an application by selecting File | New. In the New Gallery window, select General in Categories and Application in Items, and click on OK. In the Create Application window specify the application name, select application template as no template and click on OK. In the Create Project window, specify the project name and click on ok. An application and a project are added to JDeveloper. Add a JSP to the project. Select File | New and in the New Gallery window, select Web Tier | JSP. In the Items, select JSP and click on ok. Click on next in the Create JSP Wizard. Select J2EE 1.4