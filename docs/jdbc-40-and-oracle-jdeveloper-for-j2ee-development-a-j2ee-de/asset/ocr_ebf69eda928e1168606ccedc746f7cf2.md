The web application is deployed to the WebLogic Server, and the output in JDeveloper is shown below:

```
Deployment started.
Target platform is WebLogic Server 9.x (WebLogicConnection).
Checking weblogic.xml for completeness...
Wrote WAR file to C:\JDeveloper\jdev\mywork\WebLogicApp\WebLogic\deploy\webapp1.war
Wrote EAR file to C:\JDeveloper\jdev\mywork\WebLogicApp\WebLogic\deploy\webapp1.ear
C:\JDeveloper\jdk\jre\bin\javaw.exe -classpath
C:\wls\weblogic91\server\lib\weblogic.jar weblogic.Deployer -adminurl t3://localhost:7001 -user weblogic -password **** -debug -verbose -deploy -upload -source
C:\JDeveloper\jdev\mywork\WebLogicApp\WebLogic\deploy\webapp1.ear -name webapp1
weblogic.Deployer invoked with options: -adminurl t3://localhost:7001 -user weblogic -debug -verbose -deploy -upload -source
C:\JDeveloper\jdev\mywork\WebLogicApp\WebLogic\deploy\webapp1.ear -name webapp1
[WebLogicDeploymentManagerImpl.<init>():103] : Constructing DeploymentManager for J2EE version V1_4 deployments
[WebLogicDeploymentManagerImpl.getNewConnection():146] : Connecting to admin server at localhost:7001, as user weblogic
[ServerConnectionImpl.getEnvironment():282] : setting environment
[ServerConnectionImpl.getEnvironment():285] : getting context using t3://localhost:7001
[ServerConnectionImpl.getMBeanServer():237] : Connecting to MBeanServer at service:jmx:t3://localhost:7001/jndi/weblogic.management.mbeanservers.domainruntime
[ServerConnectionImpl.getMBeanServer():237] : Connecting to MBeanServer at service:jmx:t3://localhost:7001/jndi/weblogic.management.mbeanservers.runtime
[DomainManager.resetDomain():36] : Getting new domain
[DomainManager.resetDomain():39] : Using pending domain: false
[MBeanCache.addNotificationListener():96] : Adding notification listener for weblogic.deploy.api.spi.deploy.mbeans.TargetCache@1827d1
[MBeanCache.addNotificationListener():103] : Added notification listener for weblogic.deploy.api.spi.deploy.mbeans.TargetCache@1827d1
[MBeanCache.addNotificationListener():96] : Adding notification listener for weblogic.deploy.api.spi.deploy.mbeans.ModuleCache@8f9a32
[MBeanCache.addNotificationListener():103] : Added notification listener for weblogic.deploy.api.spi.deploy.mbeans.ModuleCache@8f9a32
[ServerConnectionImpl.initialize():170] : Connected to WLS domain: wl_server
```