With that, we have successfully configured the SonarCloud project. Now, let’s analyze the project in the most basic way by following these steps:

+   1. In SonarCloud Project, under **Configure**, choose the recommended option, which is **With GitHub Actions** (as we learned in the previous section, _Getting started with GitHub Actions_):

Figure 16.26 – SonarCloud – using a GitHub Actions template

+   2. Then, follow the indications provided by SonarCloud to create GitHub secrets (with the `SONAR_TOKEN` secret) inside the GitHub repository and add a new file with some Sonar configuration:

Figure 16.27 – SonarCloud GitHub Actions instructions