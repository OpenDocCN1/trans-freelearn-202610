
chronicle ID. Again check it out and check it back in. The server again assigns a new object ID but retains the original chronicle ID.

Refer to Figure 2.12 to understand how Content Server manages different versions of the original document. For the original (root) version of the document, the value of object ID and chronicle ID is same.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_40_0.png)

Figure 2.12: Relationship between object ID and chronicle ID

## 2.11 Lifecycles

Documents in an enterprise progress through a well-defined authoring-review process.

Let us take an example of such a process:

- 1. A document is created by an author. At this initial stage of creation, the document is in the draft state.
- 2. The document is being worked upon by the author and hence falls under the 'Work in progress' state.
- 3. When it is sent out for the first round of review, it might be under the 'In Review' state
- 4. And when the document is finally approved, it falls under the 'Approved' state (say).

Content Server provides lifecycles to automate these various stages in the life of a document.

Simply speaking, a lifecycle is a sequence of states that describe the various stages in the life of an object. Documentum stores lifecycles in the form of a policy object (dm_policy) in the Docbase.

Figure 2.13 shows a sample lifecycle as it is seen in Documentum Application Builder.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_41_0.png)

Figure 2.13: Documentum test lifecycle

We will delve deeper into lifecycles in Chapter 12.

## 2.12 Workflows

In plain terms, a workflow models a business process. The following example can explain this better:

An online newspaper publishing firm requires various approvals of the created content, before publishing over to its live newspaper website. Instead of manually transferring hard copies of content to various groups of reviewers, a workflow can formalize, standardize, and automate the whole process.

Workflows consist of numerous activities, each comprising various tasks to be performed. Users or designated automated scripts carry out the tasks and pass over the document in question to the subsequent activity. What a workflow does, in essence, is routing the content through the various stages of its lifecycle via different users. Each user receives the designated task in one's Inbox and may also receive an email notification for the same. Documentum stores workflow definitions in the form of business process objects ('dm_process') in the Docbase.

Figure 2.14 shows a sample Documentum workflow template as seen in Workflow Manager tool.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_42_0.png)

Figure 2.14: A sample Documentum workflow

We shall cover workflows in detail later in Chapter 19.

This concludes our discussion of Documentum basics. Please do not be bogged down if you still do not understand much of the topics covered in this chapter. As and when you start putting your hands into the Documentum system, you will understand these concepts better and appreciate them more. If you feel that you have a basic understanding of Documentum now, you may please proceed to the next chapter to cover some of the advanced features in Documentum.

## 2.13 Summary

In this chapter we talked about Content Server, the backbone of Documentum architecture, hosting Docbases. A Docbase can be thought of a logical repository, consisting of content files and their associated metadata. A DocApp, on the other hand is a packaging unit of a Docbase, consisting of objects from the Docbase.

We went on to discuss DocBrokers, which act as an intermediary allowing clients to make connections with Content Servers.

We saw how the Documentum system treats all entities in the form of objects. Objects can be treated as run-time instances of templates called object types in Documentum. Objects in Documentum have their own metadata, which is termed properties or attributes.

We saw an example of using DQL (Document Query Language) and Server API commands to query and manipulate objects in the Docbase.

We also touched upon the versioning capabilities of Content Server and had a brief introduction to lifecycles and workflows. While lifecycles represent the logical stages in the life of an object, workflows model a business process and define how the objects move into different lifecycle states, under what conditions, and who the participating entities are.

## 3 Documentum Advanced Concepts

We can now proceed further with the assumption that you have brushed up your basic concepts about the Documentum system. In this chapter, we will discuss some more vital aspects of Documentum architecture. Needless to say, you may skip this chapter and move on if you already understand the concepts listed below.

Let us not forget that the Documentum system is extremely vast and is not just limited to the basic/ advanced concepts we discuss.

> For an exhaustive understanding of the system, going through Documentum's product manuals is strongly recommended.

### 3.1 DMCL

DMCL stands for Documentum Client Library which provides a communication layer (RPC) between end clients and the Content Server.

Client requests reach the Content Server via the DMCL layer, which consists of a C++ library of APIs that get executed on the Content Server.

It is recommended not to execute direct DMCL API calls from the client. Instead, what should be used is a set of Documentum Java classes or a DFC, described below.

### 3.2 DFC

DFC stands for Documentum Foundation Classes and is an Object-Oriented (OO) Framework consisting of APIs for accessing and extending Content Server functionality. Simply speaking, it is a set of Java interfaces and classes written as a wrapper over DMCL. Apart from the regular DMCL capabilities, DFC also provides features such as data validation, virtual document management, etc.

Additionally, DFC also provides a Documentum Java-COM Bridge (DJCB) so that the Java interfaces can be used from within a Microsoft COM environment as well as by programs in Visual Basic, for example. DFC also includes a Business Objects Framework (BOF) that enables developers to model application business rules in the form of reusable modular components/elements.

Figure 3.1 shows how the request-response paradigm works between clients and Content Server via the DMCL and DFC layer.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_45_0.png)

Figure 3.1: DFC-DMCL architecture

We will learn more about DFC through numerous examples in Chapter 23.

### Documentum 5.3 Update

Documentum release 5.3 includes some enhancements made to the DFC. Some of the new features introduced in DFC as part of release 5.3 are:

- Support for Unified Client Facilities (UCF) to assist content-related services such as check-out, check-in, import, and export as the content transfer mechanism for WDK-based applications
- Deeper integration with .NET

### 3.3 BOF

The Business Objects Framework (BOF) is built into DFC and helps developers create reusable business logic components called Business Objects. Applications using DFC can access BOF easily and can use BOF as pluggable components/entities in middle-tier or client applications. Let's take a simple example to understand this.

Assume that you require a specific business functionality of performing some particular validation whenever a particular document of some particular object type is checked-in into the Docbase. This can be achieved by using BOF and overriding the check-in functionality for the particular object type. Once written, the same business object is called irrespective of the client application used—be it Webtop, Web Publisher, some Desktop application, or some custom DFC class for that matter. In short, what we achieve is a reusable component/functionality.

There are two types of Business Objects:

- **Type-based (TBO)**: This is useful in extending a particular Content Server object type and including new custom methods or overriding existing methods. This type of Business Object works on all objects in the Docbase of a particular object type only.
  Example: Some custom behavior added for an object type `my_custom_document` (where `my_custom_document` object type extends Documentum `dm_document` object type)
- **Service-based (SBO)**: Unlike type-based Business Objects, this type of Business Object is not limited to a particular Docbase object type. Instead this works as a generic service provided to all object types in the Docbase and not just one particular object type.
  Examples: Check-in method overridden globally in the Docbase irrespective of the object types, Documentum Inbox object, etc.

Note: For those coming from an EJB (Enterprise Java Beans) background, consider TBOs analogous to Entity Beans and SBOs analogous to Session Beans.

### Documentum 5.3 Update

In Documentum release 5.3, TBOs are associated not just with object types but also with a Docbase (repository). Additionally, Documentum has added support for accessing Business Objects via Web Services in this release.

### 3.4 WDK

Web Development Kit (WDK in short) is a framework provided by Documentum, on which one can build a web application for talking to the Content Server.

The WDK programming model is completely based on J2EE and XML and WDK applications can be deployed on any application server that conforms to J2EE standards. WDK applications interact and fetch data from the Content Server via DFC calls.

Following are some features of the WDK framework:

- Consists of reusable and configurable components that generate HTML controls/widgets, providing access to the Content Server Docbase
- Provides a tag library of configurable UI controls
- Provides framework services like branding, history, error handling, messaging, internationalization, and content transfer
