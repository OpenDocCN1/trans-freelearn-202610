

+   Google contacts/calendar connectors: Google offers a long list of quality web-based services aimed at helping both individuals and businesses of all sizes. Both Google Calendar and Google Contacts are attractive options for smaller-sized businesses because they are easy to use and ubiquitous. The latter also makes it a perfect extension for CRM systems, and these connectors aim at allowing you to extend your CRM data to these Google services.

+   Microsoft Outlook connector: Integration with Microsoft Outlook is critical to many organizations. These utilities allow you to synchronize data between Outlook and your SugarCRM deployment. If you prefer to use Outlook as your primary e-mail client, you will want to give this a look.

+   Microsoft Office integration: The prior chapter showed you some of the e-mail marketing capabilities that are a part of SugarCRM. However, many organizations do not exclusively use e-mails for their marketing activities. Direct mail campaigns are another popular form of marketing products and services, usually leveraging Microsoft Office to produce the piece. Office integration components allow you to create form letters in Microsoft Word and execute mail merge leveraging your customer information in SugarCRM.

+   ZuckerReports: This is a must for all Community Edition users. Given that Community Edition does not include a reporting module, you would only be able to generate reports using other tools, such as Crystal Reports, MS-SQL Reporting Services, and others. However, none of those tools are ready for SugarCRM and require intimate knowledge of the database. ZuckerReports offers a streamlined solution to reporting needs and is tightly integrated into SugarCRM.

+   Development toolkit & enhanced studio: Not all enhancements are specifically targeted at addressing end user needs or day-to-day requirements. Some of them are geared towards admin level tasks or simplifying the customization process. These tools are great examples of this, as they provide simple methods that make the process of applying more advanced customization features far easier.

+   Open quotes and contracts: This is an important functionality that is not included in SugarCRM Community Edition, but is critical to many sales organizations. This enhancement brings this much needed functionality to Community Edition users.

+   VeryThinClient: Provides access to SugarCRM data through wireless devices, such as smartphones or connected PDAs.

+   Security suite: This enhances the security model in SugarCRM to permit the assignment of data to multiple users, or teams, providing greater flexibility over data access.

Additionally, we will examine important aspects of the Professional and Enterprise Editions of SugarCRM that differentiate them from the Community Edition. This comparison should help you make a more informed decision regarding the more appropriate edition for your organization.

+   SugarCRM Professional/Enterprise Edition
    +   Product catalog and products module: These two modules within Sugar Pro provide a product catalog defining the products handled by your business, and a products module that lists the physical products in your inventory, on order, or the ones that are sold to clients.
    +   Quotes module: This Sugar Pro module uses data from the product catalog module to prepare customer quotations.
    +   Forecasting: This module within Sugar Pro provides the ability to generate individual and roll-up sales forecasts.
    +   Standard and custom reporting: Reporting is likely to be the single most important feature that differentiates Sugar Pro from the Open Source version. The reporting module in Sugar Pro gets better with each release, and is now a very capable system.
    +   Sales teams: This is a key feature of Sugar Pro. Sales Teams provides you with the ability to create teams of users who can share data between themselves that other users may not access. Combined with the Access Control Lists from release 4.0, they provide a good solution to data security.

At the end of this chapter, we also have a section about the SugarCRM online forums where users can interact and exchange ideas with other SugarCRM users and administrators.

Let us dive right in to these many and varied extensions to the SugarCRM Community Edition product. By the time we are done, you will be surprised and impressed at the range and capability of the features available to you, to enhance your SugarCRM Community Edition system that will help meet your evolving business needs.

Configuration management, or the tracking and documenting of modifications applied to the system becomes an important practice at this point. Since the enhancements that we will be exploring will be modifying your SugarCRM installation, it is vital to highlight the importance of documenting the changes that have been made as a general best practice. This will help pinpoint the source of any problems, or unintended changes in functionality that may arise through the installation or use of modules or other modifications.

As we go through the various add-ons, we will see how they relate to a small business, such as RayDoc, and discuss the real business value that they offer.

## SugarCRM Community Edition

SugarCRM has gained a lot of popularity because of the fact that it is a very cost-effective solution for many small or medium-size businesses. Its value has been greatly enhanced by the large community of users who have followed SugarCRM's lead and have also provided a number of product enhancements and add-ons for free. However, where exactly do you find them?

## SugarForge.org and SugarExchange.com

Open your web browser and navigate to `http://www.sugarforge.org`. On this website you will find dozens of projects that can be freely downloaded and installed onto your SugarCRM system. Take a moment to browse around and familiarize yourself with the various projects that are available. Be sure to bookmark the website for your future reference. You might notice that the various projects highlighted in the previous section of this chapter are all freely available for download on SugarForge.

Make a note of the details pertaining to the various projects. For example, the versions of SugarCRM that they support and other details. Some projects, while popular, do not support more current versions of SugarCRM. Overlooking this detail might cause you some frustration and aggravation when attempting to install it, hence you should be quite diligent in your selection.

Another website is SugarExchange.com that can be accessed by using the URL http://www.sugarexchange.com. It contains a variety of other enhancements and add-ons, or in some cases, more feature-rich versions of projects that are already available on SugarForge.org. It follows a model similar to the one that SugarCRM (the company) employs for its offerings. The main difference between the two websites is that SugarExchange.com is intended to be a catalog of commercial add-ons and enhancements for SugarCRM. Take a look at some of the items in the catalog and you will notice that they require purchase, unlike the items in SugarForge.org. Furthermore, you will notice that many of the projects are also not intended for use with SugarCRM Community Edition, as they are only used with the Professional and/or Enterprise Edition.

We will assume that the installation files corresponding with the various enhancements and modules to be examined have already been downloaded. As we progress through the rest of this chapter, we will learn how these installation files are used through the **Module Loader** tool, and then examine the enhancements in greater detail.

## Module loader

Before we dive into the functionality provided by the various modules and enhancements, we need to familiarize ourselves with the typical installation process.

Locate the **Enhanced Search** project on SugarForge.org and download the most current installation files. Save them onto a convenient location as we will use them in the following exercise to highlight the usage of the Module Loader tool.

You may have noticed that the installation files are contained in a ZIP archive. Do not unzip these files unless the documentation for the projects specifically instructs you to do so.

Our first step in the installation process will be to access the **Module Loader** tool found within the Admin control panel in SugarCRM. To access it, login to SugarCRM as an admin level user and then click on **Admin | Module Loader**.

