DD can support multiple locales and hence one can localize the stored information as per the default locale of various end users. Also, DD information is managed internally by Content Server and can be made available to end users once we publish the data it contains.

At the time of writing this book, the DD information for the following locales is provided by Documentum when the Content Server is installed:

+   - English
+   - French
+   - German
+   - Italian
+   - Japanese
+   - Korean
+   - Spanish

# 3.12 Methods and Jobs

Methods are executable programs and scripts in Docbase that can be scheduled to run automatically by jobs, which are Documentum objects (dm_job) that are used to automatically execute method objects. The following sections discuss methods and jobs in detail.

# 3.12.1 Methods

Method scripts can be written either in Java, Docbasic (Documentum's proprietary language), or some other languages. In Documentum, a method is treated as a method object (dm_method) and its attributes define various method parameters such as execution agent, method type, timeout period, and method arguments. Methods can be executed by the following server execution agents:

+   - **Content Server**: This is the default execution agent for methods unless a specific execution agent is specified. Content Server is used to execute method objects associated with Docbasic scripts or scripts in other programming languages.
+   - **Method Server**: This is a daemon process always running while the Docbase is up and is managed by the Content Server. Note that the Method Server is installed by default along with the Content Server. Method Server can be used to execute method objects associated with Docbasic scripts.
+   - **Java method server**: During Content Server installation, Documentum installs Apache Tomcat application server as a Java method server on the Content Server host. The Java method server can be used to execute method objects that call Java methods in turn.

Documentum methods can be created, modified, and viewed from within Documentum Administrator at the following location: Administration | Job Management | Methods.