
Salesforce asks you to perform an automated security scan of your software via a web page (http://security.force.com/security/tools/forcecom/scanner). This service can be quite slow depending on how many scans are in the queue. Another option is to obtain the Eclipse plugin from the actual vendor CheckMarx at http://www.checkmarx.com, which runs the same scan but allows you to control it locally. Finally, for the ultimate confidence as you develop your application, Salesforce can provide a license to integrate it into your Continuous Integration (CI) build system. CI is covered in the final chapter of this book.

This book focuses on building a fully native application, as such additional work involved with so-called "hybrid" applications (where parts of your application have been implemented on your own servers, for example) are not considered here. However, keep in mind that if you make any callouts to external services, Salesforce will also most likely ask you and/or the service provider to run a BURP scanner, to check for security flaws.

Make sure you plan a reasonable amount of time (at least two to three weeks, in my experience) to go through the security review process; it is essential that you initially list your package. Though if it becomes an issue, you have the option of issuing your package install URL directly to initial customers and early adopters.

## Getting the best out of the Partner Community

It's worth taking some time to review the content and facilities in the Partner Community. Some of the key areas to take a look at are listed as follows:

- Education and Trailhead: This allows you to monitor the progress of other users in your organization on Trailhead. Trailhead is Salesforce's way of learning while doing. Users read about new technologies or development approaches and are then asked to perform some challenges to validate their understanding. They are awarded badges, as part of a gamification system. Using this tab, you can see who has the most badges!

- Featured Groups: This page, under More, allows you quick access to a number of Salesforce-managed Chatter groups. A key group is the Partner Alerts group, I would strongly recommend you set up an E-mail digest for this group. Only Salesforce posts to this group, so a per-post digest level is tolerable and keeps you informed without having to log in to the community.

- Support: This is, of course, the place you go to raise cases with Salesforce. As you raise cases, the UI automatically attempts to search for known issues or support articles that might help answer your questions. You can also report and filter on open cases here.

- **Publishing**: This page allows you to list your creations on the Salesforce AppExchange site. Later sections in this chapter cover this in more detail.

- **Partner Alerts**: Partner Alerts are critical to keeping on top of changes to the service that could affect your development process and/or your customers. These might range from critical fixes, security improvements, or changes in behavior you need to be prepared for. Although rare, you may be asked to make changes to your solution by a certain deadline to ensure your users are not impacted.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_51_0.png)

There are many Chatter groups shown in the Featured Groups page in the Partner Community. Review them all and set up E-mail digests to help keep you informed without having to manually log in and check through this page each time.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_51_1.png)

## Creating test and developer orgs via Environment Hub

Partners can use Environment Hub to create orgs for development and testing purposes. Orgs can be linked and logins managed here as well. Unlike the orgs you can get from the Salesforce Developer site, these orgs have additional user licenses. It is also possible to link your TSO and create orgs based on templates you define, allowing you to further optimize the base org configuration for your own development and testing process. For more information, review the Partner Community page detailing Environment Hub (https://partners.salesforce.com/s/education/general/Environment_Hub).

## Introduction to AppExchange and listings

Salesforce provides a website referred to as **AppExchange**, which lets prospective customers find, try out, and install applications built using Force.com. Applications listed here can also receive ratings and feedback. You can also list your mobile applications on this site as well.

> In this section, I will be using an AppExchange package that I already own. The package has already gone through the process to help illustrate the steps that are involved. For this reason, you do not need to perform these steps at this stage in the book; they can be revisited at a later phase in your development once you're happy to start promoting your application.

Once your package is known to AppExchange, each time you click on the **Upload** button on your released package (as described previously), you effectively create a private listing. Private listings are not visible to the public until you decide to make them so. It gives you the chance to prepare any relevant marketing details and pricing information while final testing is completed. Note that you can still distribute your package to other Salesforce users or even early beta or pilot customers without having to make your listing public.

In order to start building a listing, you need to log in to Partner Community and click the Publishing tab in the header. This will present you with your Publishing Console. Here, you can link and manage Organizations that contain your Packages, create Listings and review Analytics regarding how often your listings are visited.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_53_0.png)

Select the Publishing Console option from the menu, then click on the Create New Listing button and complete the steps shown in the wizard to associate the packaging org with AppExchange; once completed, you should see it listed.

It's really important that you consistently log in to AppExchange using your APO user credentials. Salesforce will let you log in with other users. To make it easy to confirm, consider changing the user's display name to something like MyCompany Packaging.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_53_1.png)

Although it is not a requirement to complete the listing steps, unless you want to try out the process yourself to see the type of information required. You can delete any private listings that you created after you complete this book.

## Downloading the example code

You can download the example code files for all Packt books that you have purchased from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_54_0.png)

## Installing and testing your package

When you uploaded your package earlier in this chapter, you will receive an e-mail with a link to install the package. If not, review the Versions tab on the Package Detail page in your packaging org. Ensure that you're logged out and click on the link. When prompted, log in to your testing org. The installation process will start. A reduced screenshot of the initial installation page is shown in the following screenshot; click on the Continue button and follow the default installation prompts to complete the installation:

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_54_1.png)

Package installation covers the following aspects (once the user has entered the package password, if one was set):

- **Package overview:** The platform provides an overview of the components that will be added or updated (if this is an upgrade) to the user. Note that, due to the namespace assigned to your package, these will not overwrite existing components in the subscriber org created by the subscriber.

- **Connected App and Remote Access:** If the package contains components that represent connections to services outside of Salesforce services, the user is prompted to approve these.

- **Approve Package API Access:** If the package contains components that make use of the client API (such as JavaScript code), the user is prompted to confirm and/or configure these. Such components will generally not be called much; features such as **JavaScript Remoting** are preferred, and they leverage the Apex runtime security configured post-installation.

- **Security configuration:** In this step, you can determine the initial visibility of the components being installed (objects, pages, and so on), selecting admin only or the ability to select profiles to be updated. This option predates the introduction of **permission sets**, which permit post-installation configuration.

> If you package profiles in your application, the user will need to remember to map these to the existing profiles in the subscriber org, as per step 2. This is a one-time option, as the profiles in the package are not actually installed, only merged. I recommend that you utilize permission sets to provide security configurations for your application. These are installed and are much more granular in nature.

When the installation is complete, navigate to the **Installed Packages** menu option under the **Setup** menu. Here, you can see the confirmation of some of your package details, such as namespace and version, as well as any licensing details, which will be discussed later in this chapter.
