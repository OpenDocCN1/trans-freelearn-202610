# # Oracle WebCenter Spaces installation

Oracle WebCenter installation is detailed on the official website of Oracle:
http://www.oracle.com/technology/products/webcenter/index.html
Consider the following points during installation:

+   • It is necessary to have an Oracle database version 10.2.0.4 and higher or
+   11.1.0.7 and higher. However, with Patchset 1 it’s possible use MS SQL Server
+   and IBM DB2.
+   • You must run the **Repository Creation Utility (RCU)** to create the schemas
+   required for WebCenter Spaces to work.
+   Both products can be downloaded from: http://www.oracle.com/
+   technology/software/products/middleware/htdocs/fmw_11_
+   download.html.

> > We recommend following the steps found in the guide: Oracle
> Fusion Middleware Quick Installation Guide for Oracle WebCenter.

# # Oracle Spaces configuration

Before initiating the creation of personal and group spaces, the administrator needs
to configure Repository Services Content, Discussions, Wikis, and so on.
In this case, as administrators we will set up access from WebCenter Spaces to a
folder that contains files for each user. This folder will have the user name and
will be in the file system of the server.
1. Go to the Enterprise Manager console: http://localhost:7001/em.
2. Click on the WebCenter Spaces service.

![](bbox=[0.388, 0.653, 0.608, 0.831])