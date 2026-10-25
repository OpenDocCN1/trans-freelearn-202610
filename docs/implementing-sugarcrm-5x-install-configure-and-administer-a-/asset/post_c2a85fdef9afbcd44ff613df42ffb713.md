
## Preface

SugarCRM is a popular **Customer Relationship Management (CRM)** system. It is available in both free open source and commercial versions, making it an ideal way for small or medium-size businesses to try out a CRM system without committing large sums of money. Although SugarCRM is carefully designed for ease of use, attaining measurable business results requires careful planning and research. This book distills hard won SugarCRM experience into an easy-to-follow guide to implementing the full power of SugarCRM. SugarCRM is an extensive PHP/MySQL based application, but with its rich administration interfaces, no programming is required to get measurable benefits from its use.

## What this book covers

_Chapter 1, Doing Business – Better_, discusses the business size classifications and their varying technology needs. We also discuss the benefits of using CRM technology and systems, the definition of a CRM system, available CRM options and deployment models, customization considerations and their importance to a CRM system, an overview of SugarCRM and what makes it an effective CRM solution.

_Chapter 2, One Size Does Not Fit All – CRM Your Way_, goes through the process of analyzing the CRM needs of specific businesses, and uses the RayDoc case study as an example of how to match CRM capabilities to the needs of a business and focus primarily on the business objectives and benefits sought from the implementation of a CRM system. We also address the practical issues of how to get your CRM customized, what sorts of partners to look for to help you in that process, how to document your customization requirements, and how to manage a customization contract.

Chapter 3, CRM Deployment Options, takes a systematic approach to making the right deployment choices for your business. We take a detailed look at the alternatives available for a CRM deployment including On-Demand, Collocation, On-Premise, and a Shared Server hosting option for small organizations. We also discuss some important points to be considered while deploying a CRM system in detail, such as selecting an operating system (Windows Server or Linux), hardware configuration of the server, the required infrastructure, data backup and security concerns, and the Internet bandwidth.

Chapter 4, SugarCRM Basics, takes a guided tour of SugarCRM, covers the basic CRM functions, and shows how they are interpreted and performed within SugarCRM. We also discuss the basic CRM concepts, such as common CRM processes and terminology, creating accounts and contacts, relating multiple contacts to a single account, following links between related data to get a full view of a customer, creating and tracking the Sales Pipeline from leads to opportunities to contacts and accounts, creating and monitoring sales activities and accumulating activity history, and scheduling activities with colleagues by referring to their calendars.

Chapter 5, Extending The Business Role of Your SugarCRM System, deals with the expansion of the role of your CRM system within your business, beyond that of basic CRM functions. We cover removing the sample data and resetting the database to eliminate the default data and creating and running e-mail marketing campaigns.

Chapter 6, The SugarCRM Ecosystem, explores a number of free add-ons for SugarCRM Community Edition that can extend it even further in a myriad of directions. Along the way we will also take a look at the process of using the Module Loader tool and the benefits of upgrading to Professional or Enterprise Edition.

Chapter 7, Managing Your CRM Implementation, discusses some of the real-world challenges of introducing a new CRM solution into an organization. The goal of this chapter is to leave you with a good understanding of how to approach and structure your own CRM implementation project. You will discover that one of the keys to a successful CRM implementation is to approach it in a step-wise manner, we therefore focus on some important topics, such as key steps to a successful CRM implementation, the CRM training process, and continuous feedback and enhancement.

Chapter 8, Linking Your Customers to Your SugarCRM, discusses important topics, such as capturing customer leads and requests for information from the public website directly within the CRM system, efficiently tracking customer service requests, and related product/service flaws to improve customer satisfaction. We also discuss developing a customer self-service portal in conjunction with the CRM system to allow clients to file or monitor their own service cases.

Appendix A, Installing SugarCRM on Linux, discusses the steps required for the installation of SugarCRM on CentOS Linux server. It also discusses the installation of CentOS Linux server and the configuration of PHP and Apache web server.

Appendix B, Installing SugarCRM on Windows Server, provides all the necessary steps for installing SugarCRM on a Windows server. It also provides guidelines for installing SugarCRM with Microsoft SQL server and Internet Information Services (IIS).

Appendix C, Data Import and Export, provides the guidelines for importing and exporting data from accounts and contacts. It also discusses the insertion and extraction of data from the CRM system through import and export tools when the role of the CRM tool grows within an organization.

Appendix D, The System Administrator Role, discusses the functions and responsibilities of a System Administrator for a SugarCRM installation, covering installation time and post installation topics.

Appendix E, Customizing SugarCRM, discusses the techniques for customizing SugarCRM using various customization tools, such as the Studio, Module builder, Logic hooks, Dropdown Editor, and Model-View-Controller (MVC).

Appendix F, A Word About SugarCRM 6.0, briefly discusses SugarCRM 6.0 and how it is different from SugarCRM 5.x and its benefits.

## What you need for this book

You need a basic understanding of computing and database terms. The book also assumes a certain level of proficiency with common computing tasks, such as browsing the file system, downloading files, and so on. More advanced tasks, such as manipulating your firewall, DNS server (if necessary), or adjusting security are not discussed and may require the assistance of a qualified professional.

To follow the examples, you will need access to a server upon which you can install Linux, Windows, and/or other necessary components, such as PHP, Apache, and MySQL.

## Who this book is for

If you are a small or medium-sized business owner/ manager with reasonable IT skills, a system implementer, or a system administrator who wants to implement SugarCRM for yourself as either a first CRM or as a replacement for existing solutions, this book is for you. Existing SugarCRM users who want to broaden their understanding of the topic will find this book valuable too. No programming knowledge is required to use this book.

## Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

Code words in text are shown as follows: " Unzip the contents of the installation package to the folder named C:\Program Files\Apache Software Foundation\ Apache2.2\htdocs."

A block of code is set as follows:

```
<?php
$dictionary['Account']['indices'][] = array('name'
=>'idx_custom', 'type'=>'index',
'fields'=>array('custom_c'));
?>
```

Any command-line input or output is written as follows:

```
/echo "0,10,20,30,40,50 * * * * cd </path-to-sugar>;</path-to-php> ./
scheduler.php" | crontab -u apache/
```

**New terms** and **important words** are shown in bold. Words that you see on the screen, in menus or dialog boxes for example, appear in the text like this: "we also see a few buttons just above the main panel — **Edit**, **Duplicate**, **Find Duplicates**, and **Delete**".

![](img/7ccaf60360d7c8e809d336edbba2263d_19_0.png)

![](img/7ccaf60360d7c8e809d336edbba2263d_19_1.png)
