# How to do it...

Let's change a port on which Jenkins runs:

+   1. If you run Jenkins using the command line, then you can execute a command such as `java -jar -httpPort=9999 jenkins.war` to change the existing port from `8080` to `9999`.
+   2. Another way to change the port while Jenkins is installed using the Windows package is as follows:
+      - Go to the `Program Files/Jenkins` directory where you installed Jenkins
+      - Open the `Jenkins.xml` in the editor
+      - Find `"--httpPort=8080"` and replace the port `8080` with the new port number