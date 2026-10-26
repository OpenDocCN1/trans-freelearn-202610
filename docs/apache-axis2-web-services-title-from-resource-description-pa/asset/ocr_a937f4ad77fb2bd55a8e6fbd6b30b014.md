Isn't that easy and useful? Well, the obvious answer is yes, it is useful.

This is why Axis2 has introduced the same mechanism to deploy services (and modules) as well in a very convenient manner.

Let's think about a scenario where you have a service with several third-party dependencies and a number of property files. Further, assume that you do not have a J2EE-like deployment mechanism. Then what you have to do is put all those dependent JAR files and property files into the application classpath. This work is doubled if you have one or two servers, but what will happen if you are in a clustered environment with hundreds of replicates? In that case, it won't be practical to go and add the dependent JAR files and other resources into the classpath of each and every replica. So when you have the J2EE-like deployment mechanism, you do not need to worry about such issues. You can just drop the self-contained package—in this case, the service archive file—into the replicates. This definitely reduces your work and prevents common human errors as well.

The internal structure of the Axis2 self-contained package (or archive file) is as shown in the following figure. Both the Axis2 services archive and the module archive have a very similar structure, with only minor disparities.

In the case of the Axis service archive, the descriptor.xml file becomes services.xml, and in the other case, it becomes module.xml.

File extension for the Axis2 service archive is .aar and that for the module is .mar (the service archive or module archive is just a ZIP file with a changed file extension to .aar or .mar).

![](bbox=[0.37, 0.56, 0.6, 0.72])