# Config

A single code base allows you to deploy your application consistently across different environments. Any application configuration that changes between environments and is saved as constants in the code base should be managed with environment variables. This approach provides the flexibility to scale an application with different configurations as they grow. Connection strings, API keys, tokens, external service URLs, hostnames, IP addresses, and ports are good candidates for configs. Defining config files or managing configs using grouping (development, staging, or production) is error-prone and should be avoided. You should also avoid hard-coding configuration values as constants in your code base. For example, if you are deploying your application in Azure App Service, you can specify configuration values in the Azure App Service instance rather than inside the application's configuration file. The following diagram demonstrates deploying a container image to different environments with different configs:

![](bbox=[0.122, 0.387, 0.816, 0.756])

Figure 1.5 – Application deployed to different environments with different configs

A container image represents the template of a microservice and is compromised of application code, binaries, and dependencies. The container image is then deployed to the production and staging environments, along with their configurations, to make sure that the same application is running across all environments.