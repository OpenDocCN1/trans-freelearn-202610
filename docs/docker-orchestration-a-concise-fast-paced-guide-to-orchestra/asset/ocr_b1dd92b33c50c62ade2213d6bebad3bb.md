# # Enabling autobuilds

Autobuilds can be created any time after a repository is created by selecting the repository from the **Repositories** page and clicking on the **Builds** tab. Configure the build settings and click on **Save** to save the settings or **Save and Build** to save and immediately build the project.

![](bbox=[0.105, 0.232, 0.89, 0.61])

By default, a new build will be generated every time a commit is pushed to the `master` branch. Additional build rules can be created to fit the needs of the project. For example, whenever a new version tag is pushed to Git, Docker Cloud can build a new image that uses that version for the image label.