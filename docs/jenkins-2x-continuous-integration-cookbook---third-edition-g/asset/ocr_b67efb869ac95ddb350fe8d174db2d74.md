# # How to do it...

+   1. Go to the Build section and select Execute SonarQube Scanner:

![](bbox=[0.138, 0.171, 0.496, 0.472])

+   2. You can provide the location of `sonar-project.properties` or provide details directly for static code analysis.

```
```properties
# Required metadata
sonar.projectKey=SonarHTMLCSSJS
sonar.projectName=Simple HTML CSS JS project analyzed with the SonarQube
sonar.projectVersion=1.0
# Comma-separated paths to directories with sources (required)
sonar.sources=.
# Encoding of the source files
sonar.sourceEncoding=UTF-8
```
```

+   3. `sonar.sources` is the main property for static code analysis. With this property, you inform SonarQube which directory needs to be analyzed: