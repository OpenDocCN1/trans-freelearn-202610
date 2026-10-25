The configuration in `dwr.xml` also includes a create entry for each remoted Java class. We can use singleton, new, or some other mechanism. We can also limit which methods are allowed using the include and exclude elements in `dwr.xml`. There are also security parameters in the `web.xml` configuration file such as `allowScriptTagRemoting` and `crossDomainSessionSecurity`, which when enabled, may be a huge security risk. So make sure you understand what you are doing if you are enabling the parameters.

There is one configuration parameter that is very useful while developing a DWR application, but that is a security risk in the production (especially public) site. The following init-param, debug, and test mode can be added in the web.xml.

```
```xml
<init-param>
    <param-name>debug</param-name>
    <param-value>true</param-value>
</init-param>
```
```

When debug/test mode is enabled, DWR generates test pages (there is a sample screen in Chapter 4) that are useful tools to get to know the DWR functionality.

It is also possible to use JEE access control mechanisms and limit access to DWR itself for specific JEE roles. These are specified in the web.xml file. Within DWR, there is also a role-based access control to the methods of remoted Java objects. DWR can also be integrated with Spring Security (also known as Acegi Security), `http://www.acegisecurity.org/`, the official security projects of the Spring Portfolio. The description of Acegi and DWR integration is found on `http://java-x.blogspot.com/2007/08/handling-security-with-ajax-dwr-and.html`.

DWR is also designed so that there is no possibility of an attacker accessing and manipulating DWR core files and changing the security parameters for example.

# Minimize Risks

Security is not a trivial issue, and we could spend years in the security field and still leave a lot unfinished. As developers, our job is to make sure that security risks are minimized, and at least realize what might happen if we remote a certain Java method.

A very good example from the DWR website is that if we make a remote call to a method called `appendStringToFile()`, which appends a given string to a predefined file, then a possible attacker can send a large number of requests and fill the file system with nonsense and thus cause an unexpected server downtime. The cost of downtime may be anything from little inconvenience to millions of dollars.