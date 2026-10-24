| Step/task | Description |
|---|---|
| Restore | Restores the packages referenced in the project |
| Build | Builds the project and generates binaries |
| Test | Runs unit tests |
| Publish | Creates a ZIP package that contains the binary files of the project |
| Publish build artifacts | Defines an artifact that is our ZIP package of the application, which
we will publish in Azure DevOps, and which will be used in the
deployment release, as seen in the previous section, using a package
manager in the CI/CD process |

# Table 7.2 – Azure Pipelines steps list

9. The last important configuration of our CI pipeline is the configuration of the build trigger in the **Triggers** tab to enable CI, as shown in the following screenshot:

![](bbox=[0.117, 0.493, 0.848, 0.705])

# Figure 7.31 – Azure Pipelines: enabling CI