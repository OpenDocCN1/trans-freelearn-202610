# The History of Enterprise Java

Initially, Java was meant to run in browsers, inside so-called applets. These applets added a lot of functionality to the early browsers such as Microsoft’s Internet Explorer and Netscape’s Navigator.

Free versions of Java were supplied to several popular platforms, which aided in the rapid success of the language.

The JVM specification could be licensed by third parties, allowing them to build their own implementations of the compiler and the JVM. Several companies have done so, which means that there are now several vendors that offer their own implementation. The most common are (in alphabetical order) as follows:

+   - Alibaba Dragonwell
+   - Amazon Corretto
+   - Azul Zulu
+   - Bellsoft Liberica
+   - Eclipse Adoptium Temurin
+   - J9
+   - Oracle Oracle JDK and OpenJDK
+   - Redhat OpenJDK
+   - SAP SapMachine

But soon, Java moved out of the realm of browsers into the business world. It became obvious that developing business applications required additional functionality that was not part of the language.

Instead of adding this functionality to the language itself, it was decided that it would better be provided by a separate set of APIs. To avoid confusion and distinguish between Java SE and these new APIs, they were called **Java Enterprise Edition** (Java EE).

Java EE added features such as transactions, security, scalability, management of components, and concurrency. It allowed you to create dynamic web applications and provided a robust platform for distributed transactions.

# Web servers versus application servers

The terms **web servers** and **application servers** are often confused by people new to Java EE and used interchangeably, although this is not correct. In this section, we will highlight the differences between the two, as we believe that it is important to know the differences between them, as both are key components of Java EE, but each plays its own role.

Starting to understand the difference between them is best done by highlighting their goals.