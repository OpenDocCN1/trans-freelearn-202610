```
:war {
    :dev? true
    :destination "/Users/niko/Downloads/wildfly-10.0.0.Final/standalone/deployments"
    :context-path "/mutant7"}}
```

Suppose you have downloaded WildFly (or another open source Java server such as Tomcat or Jetty); the copied WAR file should trigger a live redeployment of your application if the server is running...as seen in the server logs:

```
00:17:17,688 INFO  [org.wildfly.extension.undertow]
(ServerService Thread Pool -- 65) WFLYUT0021: Registered web context: /mutant7
00:17:17,748 INFO  [org.jboss.as.server] (DeploymentScanner-threads - 2) WFLYSRV0016: Replaced deployment "mutant7-dev.war" with deployment "mutant7-dev.war"
00:17:17,761 INFO  [org.jboss.as.repository]
(DeploymentScanner-threads - 2) WFLYDR0002: Content removed from location /Users/niko/Downloads/wildfly-10.0.0.Final/standalone/data/content/30/2621044f4972384dc11214cf376f3525ffe9ec/content
```

Your Immutant-based web application can now virtually be running everywhere!