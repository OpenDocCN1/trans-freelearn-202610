To run the project, open a Terminal, move into the project's folder, and execute the following commands in this sequence:

```
mvn clean install
mvn package
java -jar target/dependency/jetty-runner.jar target/*.war
```

Opening a browser at http://localhost:8080, you should see the Pentaho report as in the following screenshot:

![](bbox=[0.145, 0.27, 0.855, 0.53])

![](bbox=[0.14, 0.58, 0.21, 0.63])

*As you can see, the resulting Pentaho report is exactly the same as the one we developed in Chapter 10, Subreports and Cross Tabs, using Pentaho Report Designer. In other words, here you reach the same expertise on Pentaho Report Designer, but using your Java skills in development.*