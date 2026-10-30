# Working with APIs and Services

So far, we have used PAD to enable applications without an API to be used with automated processes via their **user interface (UI)**. Even though this is the main purpose of PAD, we can also communicate with APIs in the context of UI flows to wherever is the best integration option. These possibilities close the loop and allow us to implement a complete business process exclusively in PAD without adding other mechanisms. Accordingly, in this chapter, we will look at which APIs and protocols PAD can communicate with directly and which web-based data formats PAD can handle. We will highlight the following points:

+   - Managing local **Active Directory (AD)** services and accessing local database and email services
+   - Leveraging web services with **Hypertext Transfer Protocol (HTTP)** and **File Transfer Protocol (FTP)** and working with web formats (**Javascript object notation (JSON)**, **Extensible Markup Language (XML)**, and **portable document format (PDF)**)
+   - Cryptography and CyberArk
+   - Built-in data functionality – date-time, text, compression, and Messageboxes

After studying this chapter, we will be able to use internal services such as databases and AD, as well as any web services within a UI flow, working with the various data formats. An example flow for the actions presented here will be shown in *Chapter 12*.

# Technical requirements

In the first example, I will use a locally installed SQL Server with the Microsoft sample database **AdventureWorks2019** installed. In addition, I have set up a database user on this server with access permission to this database.