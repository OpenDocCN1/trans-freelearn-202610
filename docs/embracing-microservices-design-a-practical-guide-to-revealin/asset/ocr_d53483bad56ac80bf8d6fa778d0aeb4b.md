# # Deployment strategy

A deployment strategy is a method for distributing or upgrading versions of your program on various deployment environments. Traditional applications lack the agility and speed of microservices-based solutions. Developers may make changes fast, and a new version can be issued with minimal testing. The following aspects should be considered when evaluating a deployment strategy:

+   - Have you documented a deployment strategy for deploying your services?
+   - Are you using outdated tools and technologies for deploying or hosting your services?
+   - Is there any collaboration needed with other teams when you deploy services?
+   - Are you setting up the infrastructure using IaC?
+   - Have you leveraged DevOps capabilities to automate deployments?
+   - Do you have a practice of propagating the same build to multiple environments?

# # Monitoring services

Monitoring is an important factor for any microservices-based application. Since the application is split into a set of various services running independently, troubleshooting errors becomes a daunting task. If you are following proper semantic logging for monitoring your services, your teams can easily monitor and investigate errors and respond quickly. Here are some aspects to be considered when evaluating this factor:

+   - Do you have a practice of monitoring applications once they are deployed?
+   - Is there a proper logging mechanism in place?
+   - Is there distributed tracing infrastructure in place?
+   - Is there a practice of recording exceptions?
+   - Are defined health checks implemented for each service?
+   - Are you following semantic logging?
+   - Have you implemented key metrics, thresholds, and indicators?