# Previewing the Pentaho report

Now that the web application has been updated, the last task is about compiling and running the project, as we already saw earlier in the chapter. To compile the Java web application, open a Terminal, move into the `pentaho-reporting-web-app` folder, and run `mvn clean package`. At the end of the execution of each command, a `BUILD SUCCESS` message should appear. At the end of the `package` command, the `target` folder will contain the WAR package named `pentaho-reporting-web-app.war`. To deploy the WAR package in the Eclipse Jetty server and run it, execute the following command: **java -jar target/dependency/jetty-runner.jar target/*.war**

After a few seconds and some lines of log messages, the web application will be up and running on port `8080`. The following screenshot shows what your browser should look like, opening the page `http://localhost:8080/reporting`:

![](bbox=[0.115, 0.465, 0.726, 0.857])

As you can see, the Java servlet is previewing the `my_first_report.prpt` file in PDF