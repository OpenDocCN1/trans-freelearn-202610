When running, ZAP will act as a proxy between the user and the application by scanning
all the URLs of the application, then performing a series of penetration tests on these
different URLs. It is currently one of the most widely used tools in application testing
because, in addition to being free, it provides many very interesting features, such as the
ability to configure Ajax penetration tests, as well as also advanced test configurations. In
addition, it integrates very well with many CI/CD pipeline platforms. Finally, it is possible
to control it using REST APIs. The respective documentation is available at `https://
www.zaproxy.org/docs/api/`.

What I propose to do is make a small lab using ZAP on a public demonstration website
that has security holes. As we mentioned in the _Technical requirements_ section, one
prerequisite of using ZAP is to have Java installed on the machine that will perform the
tests. It can be a local machine or a build agent.

We can download ZAP at `https://www.zaproxy.org/download/`; download the
package that corresponds to your OS.

Then, install ZAP by following the software installation procedures of your OS. Once the
installation is complete, we can open ZAP and access its interface.

The following screenshot shows the default ZAP interface:

![](bbox=[0.132, 0.488, 0.867, 0.759])

Figure 13.1 – OWASP ZAP tool

We will perform our first piece of security analysis with ZAP by following these steps:

+   1. In the right-hand panel, click on the **Automated Scan** button, which will open a
+   form where we can enter the URL to be scanned.