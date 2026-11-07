# Using OWASP ZAP to find session issues

OWASP ZAP is a fantastic, multipurpose tool that comes directly from OWASP, so it is designed by security personnel with a strong background in security and web applications. It is a great penetration tool for finding all sorts of vulnerabilities within the web application, so it should be in the toolbox of anyone who does any sort of web application testing. OWASP ZAP can perform the following tasks:

+   - Run a spider discovery of the site
+   - Act as a proxy
+   - Both active and passive scanning
+   - Change requests and responses

It's the swiss army knife of web application penetration tools and this is just a subset of what it can do.

For my lab testing in this exercise, I will be using the scanning functionality to find any session related information/vulnerabilities/issues. These are the step by step actions I took in the lab.

+   1. Once I start up OWASP ZAP, I get the default screen which shouldn't have anything in it yet. OWASP ZAP is a very powerful tool, and can be used for many web application testing procedures but in my example, I will be using it to scan for session-based vulnerabilities so that I can leverage other tools to retrieve and use that session information. Here is the default screen I am presented with:

![](bbox=[0.1, 0.59, 0.9, 0.88])