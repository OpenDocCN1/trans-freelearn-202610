
# Mule Maven Plugin

The Mule Maven plugin is an important Maven plugin for deploying, undeploying, and packaging MuleSoft applications on-premises. The following plugin can be configured in `POM.xml` of your application with all the attributes that are required for deploying and running the application on-premises:

```xml
<plugin>
  <groupId>org.mule.tools.maven</groupId>
  <artifactId>mule-maven-plugin</artifactId>
  <version>3.8.6</version>
  <extensions>true</extensions>
  <configuration>
    <standaloneDeployment>
      <muleHome>${mule.home}</muleHome>
      <muleVersion>${app.runtime}</muleVersion>
    </standaloneDeployment>
  </configuration>
</plugin>
```

In the preceding Mule Maven plugin, values defined under `${}` are the parameters that you can pass with the `mule deploy` command as follows:

```bash
mule deploy –DmuleDeploy -Dapp.runtime=4.3.0 -Dmule.home=/opt/mule/bin
```

## On-Premises Deployment Attributes

The following are the attributes that can be configured on the Mule Maven plugin while deploying an application on-premises:

| Parameter | Description | Required |
| --- | --- | --- |
| `standaloneDeployment` | Top-level element. | Yes |
| `applicationName` | Specifies the application name to use during deployment. | Yes |
| `muleVersion` | The Mule version running in your local machine instance. If this value does not match the Mule version running in your deployment target, the plugin raises an exception. The Mule Maven plugin does not download a Mule runtime engine if these values don't match. | Yes |
| `muleHome` | The location of the Mule instance in your local machine. | Yes |
| `deploymentTimeout` | The allowed elapsed time, in milliseconds, between the start of the deployment process and the confirmation that the artifact has been deployed. The default value is 1,000,000. | No |
| `skip` | When set to `true`, skips the plugin deployment goal. Its default value is `false`. | No |

*Table 12.5: Mule Maven plugin attributes*

## ARM Deployment

The Mule Maven plugin supports `Anypoint Runtime Manager` (`ARM`) deployment to deploy applications on the target, such as servers, clusters, or server groups registered on Anypoint Platform. The following plugin can be configured in `POM.xml` of your application with all the attributes that are required for deploying and running the application using ARM:

```xml
<plugin>
  <groupId>org.mule.tools.maven</groupId>
  <artifactId>mule-maven-plugin</artifactId>
  <version>3.7.1</version>
  <extensions>true</extensions>
  <configuration>
    <armDeployment>
      <muleVersion>${app.runtime}</muleVersion>
      <uri>https://anypoint.mulesoft.com</uri>
      <target>${target}</target>
      <targetType>${target.type}</targetType>
      <username>${username}</username>
      <password>${password}</password>
      <environment>${environment}</environment>
      <properties>
        <key>value</key>
      </properties>
    </armDeployment>
  </configuration>
</plugin>
```

In the preceding Mule Maven plugin, values defined under `${}` are the parameters that you can pass with the `mule deploy` command as follows:

```bash
mule deploy –DmuleDeploy -Dapp.runtime=4.3.0 -Dtarget=test-cluster -Dtarget.type=cluster -Dusername=Anypoint_Username -Dpassword=Anypoint_Password -Denvironment=Sandbox
```

## ARM Deployment Attributes

The following are the attributes that can be configured on the Mule Maven plugin while deploying applications using ARM.

| Parameter | Description | Required |
| --- | --- | --- |
| `armDeployment` | Top-level element. | Yes |
| `applicationName` | Specifies the application name to use during deployment. | Yes |
| `muleVersion` | The Mule version required for your application to run in your deployment target. | Yes |
| `uri` | The Anypoint Platform URI. | No |
| `target` | The name of the server where your Mule instances are installed. | Yes |
| `targetType` | The type of target to which you are deploying. Valid values are `cluster`, `server`, and `serverGroup`. | Yes |
| `username` | Your username for the server where your Mule instances are installed. | Only when using Anypoint Platform credentials to log in |
| `password` | Your password for the server where your Mule instances are installed. | Only when using Anypoint Platform credentials to log in |
| `environment` | The environment name for the server where your Mule instances are installed. This value must match any environment configured in your Runtime Manager account. | Yes |
| `businessGroup` | The business group path of the deployment. | No |
| `businessGroupId` | The business group ID of the deployment. | No |
| `deploymentTimeout` | The allowed elapsed time, in milliseconds, between the start of the deployment process and the confirmation that the artifact has been deployed. The default value is 1000000. | No |
| `server` | The Maven server with Anypoint Platform credentials. This is only needed if you want to use your credentials stored in your Maven `settings.xml` file. This is not the Mule server name. | No |
| `properties` | Top-level element. | No |
| `skip` | When set to `true`, skips the plugin deployment goal. Its default value is `false`. | No |
| `skipDeploymentVerification` | When set to `true`, skips the status verification of your deployed app. Its default value is `false`. | No |
| `authToken` | Specifies the authorization token to access the platform. You can use this authentication method instead of setting a username and password. | Only when using an Authorization token to log in |
| `connectedAppClientId` | Specifies the Connected Apps `clientID` value. | Only when using Connected Apps credentials to log in |
| `connectedAppClientSecret` | Specifies the Connected Apps secret key. | Only when using Connected Apps credentials to log in |
| `connectedAppGrantType` | Specifies the only supported connection type: `client_credentials`. | Only when using Connected Apps credentials to log in |

*Table 12.6: Mule Maven plugin attributes while deploying with ARM*

## Deploy and Undeploy Commands

Here are the commands to deploy and undeploy an application:

```bash
mvn clean deploy –DmuleDeploy (deploy application on-premises)
mvn mule:undeploy (undeploy application from on-premises)
```

# Deploying a MuleSoft Application On-Premises Using the Anypoint Runtime Manager (ARM) REST API

MuleSoft provides the ARM API for deploying applications on cluster or server groups or servers registered on Anypoint Platform. It also provides endpoints for managing servers, clusters, and server groups and supports many other operations via the ARM REST API. Here is the full list of operations: [https://anypoint.mulesoft.com/exchange/portals/anypoint-platform/f1e97bc6-315a-4490-82a7-23abe036327a.anypoint-platform/arm-rest-services/](https://anypoint.mulesoft.com/exchange/portals/anypoint-platform/f1e97bc6-315a-4490-82a7-23abe036327a.anypoint-platform/arm-rest-services/).

The following table showcases deploying an application to the target using the ARM API:

| Method | POST |
| --- | --- |
| API endpoint | `https://anypoint.mulesoft.com/hybrid/api/v1/applications` |
| Request body | ```json { "applicationSource": { "source": "EXCHANGE", "groupId": "com.mulesoft.hybrid", "artifactId": "platformApp", "version": "0.0.1" }, "targetId": 101, "artifactName": "test-app" } ``` |
| Authentication | OAuth 2.0 (Connected Apps or Anypoint username and password) |
| Header | `X-ANYPNT-ORG-ID` (organization ID of Anypoint Platform), `X-ANYPNT-ENV-ID` (environment ID of Anypoint Platform), and `Content-Type` (`application/json`) |

*Table 12.7: ARM API*
