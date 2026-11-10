
Imagine there is a permission set (ACL) assigned to an object and specific users have access to it. For example, all the users in the HR department have WRITE access to it and users in the Finance department have READ rights.

If we were to design the ACL giving specific rights individually to each of the users in HR and Finance departments, we would end up in a mess. We would have to revisit the ACL whenever a new user joins the HR or Finance department.

Groups save us this pain. Instead of individually assigning rights to users, we club all HR users as an HR group and all Finance users as a Finance group and then assign rights at the group level and not to specific users. Groups can be of two types:

- **Public:** If a sysadmin or a superuser creates a group, the group is marked as public.
- **Private:** If a user with 'Create group' privilege creates a group, the group is marked as private.

Internally Documentum does not treat these two types of groups differently. It is up to the individual applications to interpret it as per the business needs.

Groups can be created via the following means:

- DQL queries
- API commands
- Documentum Administrator user interface
- DFC classes

## 3.9 Renditions

Rendition of a document is pretty much the representation of the original document with differences only in its format. Content Server internally has converters to support numerous renditions. Additionally, integrating Media Services with Content Server can provide support for rich media formats as well.

An example of rendition is as follows:

A web developer creates a content template, say create_article.xml, and attaches presentation XSL files to it in order to transform it to an HTML file and a WML file. The content created from this template is rendered in the form of an HTML file as well as a WML file for display on a website and a wireless application respectively.

The HTML and WML files differ from the original content file only in format and not in their content.

Figure 3.6 explains this in a simplified fashion.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_53_0.png)

Figure 3.6: Renditions

## 3.10 Registered Tables

Most applications maintain some application tables in order to store data specific to the application. These application tables may contain records that Documentum clients need access to. For example, a dropdown in a Documentum client application might need to display a list of all the countries in the world from an external database table, so that the content author can choose one while creating content.

The external database table is not known to the Content Server until you register it. In short, registered tables are the normal RDBMS tables that the Content Server is aware of, in spite of them not being part of the Docbase.

Documentum creates a dm_registered object for the 'Registered' table, allowing developers to use DQL queries to fetch the information from the table.

The dm_registered object contains information about the name of the table, table owner, table columns, and access rights defining who has access to query the table and perform operations like insert/update/delete rows.

We shall be dealing with registered tables in greater detail in Chapter 11.

## 3.11 Data Dictionary

The Data Dictionary (DD) in the Docbase stores information about Documentum object types and their attributes. The Data Dictionary is very useful in the following scenarios:

- **Value assistance shown to the users**
  Example: A dropdown showing the possible set of values for the attribute
- **Application of some business-specific rules**
  Example: Constraints like 'not null' and 'unique key' on the values of an attribute

DD can support multiple locales and hence one can localize the stored information as per the default locale of various end users. Also, DD information is managed internally by Content Server and can be made available to end users once we publish the data it contains.

At the time of writing this book, the DD information for the following locales is provided by Documentum when the Content Server is installed:

- English
- French
- German
- Italian
- Japanese
- Korean
- Spanish

## 3.12 Methods and Jobs

Methods are executable programs and scripts in Docbase that can be scheduled to run automatically by jobs, which are Documentum objects (dm_job) that are used to automatically execute method objects. The following sections discuss methods and jobs in detail.

### 3.12.1 Methods

Method scripts can be written either in Java, Docbasic (Documentum's proprietary language), or some other languages. In Documentum, a method is treated as a method object (dm_method) and its attributes define various method parameters such as execution agent, method type, timeout period, and method arguments. Methods can be executed by the following server execution agents:

- **Content Server**: This is the default execution agent for methods unless a specific execution agent is specified. Content Server is used to execute method objects associated with Docbasic scripts or scripts in other programming languages.
- **Method Server**: This is a daemon process always running while the Docbase is up and is managed by the Content Server. Note that the Method Server is installed by default along with the Content Server. Method Server can be used to execute method objects associated with Docbasic scripts.
- **Java method server**: During Content Server installation, Documentum installs Apache Tomcat application server as a Java method server on the Content Server host. The Java method server can be used to execute method objects that call Java methods in turn.

Documentum methods can be created, modified, and viewed from within Documentum Administrator at the following location: Administration | Job Management | Methods.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_55_0.png)

Figure 3.7: wcmLifecycleMonitor method

Figure 3.7 shows a sample method wcmLifecycleMonitor as seen in Documentum Administrator.

### 3.12.2 Jobs

Instead of invoking methods manually or via scripts, jobs can be used to periodically schedule their execution. dm_job object attributes define job parameters like associated method, execution schedule, method arguments, etc.

A job is executed by the Documentum agent exec process, which is installed with the Content Server. Figure 3.8 shows how jobs and methods work together in Documentum architecture.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_55_1.png)

Figure 3.8: Jobs and methods execution

Documentum jobs can be created, modified, and viewed from within Documentum Administrator at the following location: Administration | Job Management | Jobs.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_56_0.png)

Figure 3.9: Monitor_Lifecycles job

Figure 3.9 shows a sample job (Monitor_Lifecycles) as seen in Documentum Administrator. This job in turn calls the method wcmLifecycleMonitor as seen in figure 3.10.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_56_1.png)

Figure 3.10: 'wcmLifecycleMonitor' method called by 'Monitor_Lifecycles' job

## Documentum 5.3 Update

Documentum release 5.3 introduces a new feature called Job Sequences. This allows you to configure multiple jobs to run in a sequence (in a specified order) at the specified times.

This is all we wanted to cover in the Documentum advanced concepts section for now. You will understand these concepts better as you read through the following chapters.

Treat this as a ready-to-refer chapter in case you are confused about the terminologies or feel like refreshing your concepts again. Feel free to jump back to this chapter any time you need.

## 3.13 Summary

In this chapter, we touched upon some of the advanced concepts in Documentum.

We discussed the DMCL (Documentum Client Library) as a client library serving as an RPC layer between clients and Content Server. DFC (Documentum Foundation Classes) on the other hand act as a wrapper over DMCL exposing APIs to perform basic content management functions. DFC consists of a set of Java interfaces and classes and has a built-in Business Objects Framework (BOF) that helps you create reusable business components by extending and customizing content management capabilities.

We went on to discuss the Documentum WDK (Web Development Kit), which serves as a framework consisting of several server-side reusable components, allowing us to rapidly develop and deploy web-based applications that need to talk to Content Server.

Webtop is a reference web-client implementation provided by Documentum built over WDK. Web Publisher is an extension to Webtop. It is a web browser-based application that provides a rich user interface for content creation, review, and publishing to websites.

We also talked about Access Control Lists (ACLs) that help in imposing security restrictions on Documentum objects by allowing only certain rights to specific groups or users in the Docbase.

Alias Sets are an integral part of the Documentum system, defining placeholders for various entities in the Documentum architecture so that the same objects can be reused in multiple contexts.

We saw how groups and users in Documentum are treated as objects. Users are a part of one or more groups in the Documentum system. Renditions in Documentum are documents differing from the original version only in their formats. Registered tables are RDBMS tables registered with the Docbase so that the data from these can be used within the context of a Docbase. The Data Dictionary in Documentum is a repository that stores information about all Documentum object types and their properties (attributes). Finally, we discussed methods, which are executable programs in Documentum Docbases that can be scheduled to run automatically via Documentum jobs.
