
## Building, Publishing, and Supporting Your Application

The key for turning an idea into reality lies in its execution. From having the inception of an idea to getting it implemented as an application and into the hands of users is an exciting journey, and one that constantly develops and evolves between you and your users. One of the great things about developing on Force.com is the support you get from the platform beyond the core engineering phase of the production process.

In this first chapter, we will use the declarative aspects of the platform to quickly build an initial version of an application that we will use throughout this book, which will give you an opportunity to get some hands-on experience with some of the packaging and installation features that are needed to release applications to subscribers. We will also take a look at the facilities available to publish your application through Salesforce AppExchange (equivalent to the Apple's App Store) and finally provide end user support.

We will then use this application as a basis for incrementally releasing new versions of the application throughout the chapters of this book, building on our understanding of enterprise application development. The following topics outline what we will achieve in this chapter:

- Required organizations
- Introducing the book's sample application
- Package types and benefits
- Creating your first managed package
- Package dependencies and uploading
- Introduction to AppExchange and creating listings
- Installing and testing your package
- Becoming a Salesforce partner and its benefits
- Licensing
- Supporting your application
- Customer metrics
- Trialforce and Test Drive

### Required organizations

Several Salesforce organizations are required to develop, package, and test your application. You can sign up for these organizations at https://developer.salesforce.com/. Though in due course, as your relationship with Salesforce becomes more formal, you will have the option of accessing their **Partner Portal** website to create organizations of different types and capabilities. We will discuss this in more detail later.

It's a good idea to have some kind of naming convention to keep track of the different organizations and logins. Use the following table as a guide and create the following organizations via https://developer.salesforce.com/. As stated earlier, these organizations will be used only for the purposes of learning and exploring throughout this book:

| Username | Usage | Purpose |
|---|---|---|
| myapp@packaging.my.com | Packaging | Though we will perform initial work in this org, it will eventually be reserved solely for assembling and uploading a release. |
| myapp@testing.my.com | Testing | In this org, we will install the application and test upgrades. You may want to create several of these in practice, via the Partner Portal website described later in this chapter. |
| myapp@dev.my.com | Developing | In a later chapter, we will shift development of the application into this org, leaving the packaging org to focus only on packaging. |

> You will have to substitute myapp and my.com (perhaps by reusing your company domain name to avoid naming conflicts) with your own values. You should take the time to familiarize yourself with andyapp@packaging.andyinthecloud.com.

The following are other organization types that you will eventually need in order to manage the publication and licensing of your application. However, they are not needed to complete the chapters in this book:

| Usage | Purpose |
|---|---|
| Production/CRM Org | Your organization may already be using this org for managing contacts, leads, opportunities, cases, and other CRM objects. Make sure that you have the complete authority to make changes, if any, to this org since this is where you run your business. If you do not have such an org, you can request one via the Partner Program website described later in this chapter by requesting (via a case) a CRM ISV org. Even if you choose to not fully adopt Salesforce for this part of your business, this type of org is still required when it comes to utilizing the licensing aspects of the platform. |
| AppExchange Publishing Org (APO) | This org is used to manage your use of AppExchange. We will discuss this a little later in this chapter. This org is actually the same Salesforce org you designate as your production org, and is where you conduct your sales and support activities from. |
| License Management Org (LMO) | Within this organization, you can track who installs your application (as leads), the licenses you grant them, and for how long. It is recommended that this is the same org as the APO described earlier. |
| Trialforce Management Org (TMO) and Trialforce Source Org (TSO) | Trialforce is a way to provide orgs with your preconfigured application data so that prospective customers can try out your application before buying. It will be discussed later in this chapter. |

Typically, the LMO and APO can be the same as your primary Salesforce production org, which allows you to track all your leads and future opportunities in the same place. This leads to the rule of APO = LMO = production org. Though neither of them should be your actual developer or test orgs, you can work with Salesforce support and your Salesforce account manager to plan and assign these orgs.

### Introducing the book's sample application

For this book, we will use the world of **Formula 1** motor car racing as the basis for a packaged application that we will build together. Formula 1 is, for me, the motor sport that is equivalent to enterprise applications software, due to its scale and complexity. It is also a sport that I follow. My knowledge of both of these fields helped me when building the examples that we will use.

We will refer to our application as **FormulaForce** throughout this book, though please keep in mind Salesforce's branding policies when naming your own application, as they prevent the use of the word "Force" in the company or product titles.

This application will focus on the data collection aspects of the races, drivers, and their many statistics, utilizing platform features to structure, visualize, and process this data in both historic and current contexts.

For this chapter, we will create some initial **Custom Objects** and fields, as detailed in the following table. Do not worry about creating any custom tabs just yet. You can use your preferred approach to create these initial objects. Ensure that you are logged in to your **packaging org** (we will use the development org later in this book).

| Object | Field name and type |
| :--- | :--- |
| Season__c | Name (text) |
| Race__c | Name (text)<br>Season (Master Detail Lookup to Season) |
| Driver__c | Name (text) |
| Contestant__c | Name (Auto Number CONTEST-{00000000})<br>Race (Master Detail Lookup to Race)<br>Driver (Lookup to Driver) |

The following screenshot shows the preceding objects within the Schema Builder tool, available under the **Setup** menu:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_38_0.png)

### Package types and benefits

A **package** is a container that holds your application components, such as Custom Objects, Apex code, Apex triggers, Visualforce pages, Lightning Components, and so on. This makes up your application. While there are other ways to move components between Salesforce orgs, a package provides a container that you can use for your entire application or deliver optional features by leveraging the so-called **extension packages**.

There are two types of packages – **managed** and **unmanaged**. Unmanaged packages result in the transfer of components from one org to another; however, the result is as if those components had been originally created in the destination org, meaning that they can be readily modified or even deleted by the administrator of that org. Furthermore, they are not upgradable, and are not particularly ideal from a support perspective. Moreover, the Apex code that you write is also visible for all to see, so your intellectual property is at risk.
