# # How it works...

FitNesse has a built-in set of acceptance tests that it uses to check itself for regressions. The Jenkins plugin calls the test and asks for the results to be returned in XML format using an HTTP GET request with the URL: `http://localhost:39996/FitNesse.SuiteAcceptanceTests?suite&format=xml`. The results look similar to the following:

```
```xml
<testResults>
<FitNesseVersion>v20140901</FitNesseVersion>
<rootPath>FitNesse.SuiteAcceptanceTests</rootPath>
<result>
<counts><right>103</right>
<wrong>0</wrong>
<ignores>0</ignores>
<exceptions>0</exceptions>
</counts>
<runTimeInMillis>27</runTimeInMillis>
<relativePageName>CopyAndAppendLastRow</relativePageName>
<pageHistoryLink>
FitNesse.SuiteAcceptanceTests.SuiteFitDecoratorTests.CopyAndAppend
LastRow?pageHistory&resultDate=20141101164526
</pageHistoryLink>
</result>
```
```

The Jenkins plugin then parses the XML and generates a report.

By default, there is no security enabled on FitNesse pages. In this recipe, a username and password were defined during startup. However, we did not take this further and define the security permissions on the page. To activate, you will need to go to the properties link on the left-hand side of the page and check the security permission for secure-test.

You can also authenticate through a list of users in a text file or Kerberos/Active Directory. For more details, review [http://fitnesse.org/FitNesse.FullReferenceGuide.UserGuide.AdministeringFitNesse.SecurityDescr](http://fitnesse.org/FitNesse.FullReferenceGuide.UserGuide.AdministeringFitNesse.SecurityDescr)

There is also a contributed plugin for LDAP authentication: [https://github.com/timander/fitnesse-ldap-authenticator](https://github.com/timander/fitnesse-ldap-authenticator).

Consider applying security in depth; adding IP restrictions through a firewall on the FitNesse server creates an extra layer of defense. For example, you can place an Apache server in front of the wiki, and enabling SSL/TLS ensures encrypted passwords. A thinner alternative to Apache is NGINX: [http://wiki.nginx.org](http://wiki.nginx.org).