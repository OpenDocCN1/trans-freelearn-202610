
## Performing the installation

In order not to interrupt our CRM business analysis context with a lot of procedural instructions, the actual details of installing SugarCRM may be found in Appendices A and B that cover the complete process of installing SugarCRM on CentOS Linux and Microsoft Windows network operating system environments.

## Summary

In this chapter, we covered the practical considerations of installing and running SugarCRM in your business. The key points included are as follows:

- Comparing the advantages of an On-Demand service versus a Collocated, On-Premise, or Shared Server. We also compared the up front costs of maintaining your own server versus paying monthly fees for an On-Demand service. We also explained the minimal support for customizations available with an On-Demand service or Shared Server.
- If you choose to deploy SugarCRM on your own server, you will need to select the operating system and hardware configuration for your server, and also make decisions regarding server security and Internet connection bandwidth.
- Using Windows or Linux as the server operating system on your SugarCRM server and learned about the LAMP stack. We also examined some popular variations of the LAMP stack including WAMP and WIMP.
- For security, you should consider a UPS (battery backup), a locked server closet, a firewall, VPN, and a surveillance system to protect your server from intrusions, theft or damage.
- Your offices should have an Internet connection of at least 1 megabit for both download and upload speeds. You will need about 2 megabits of download speed for every 25 employees, and about 1 megabit of upload speed for every 10 concurrent remote CRM users.
- Appendix A and B include the procedural details of the actual installation of SugarCRM within the Linux and Windows Server environments respectively.

In the next chapter, we assume that you have deployed SugarCRM and venture into using your new CRM in earnest, explaining a lot of the basic CRM terminology and concepts as we go along.

Why is a CRM system so much better than a contact manager? What are the normal navigation techniques for operating a CRM solution? How do I get my data into a CRM application? All these topics and more are coming up next.

## SugarCRM Basics

Now that we have covered some of the essentials offered in CRM systems, itemized its value to a smaller business, and studied the theoretical aspects of customizing it to properly fit each business that uses them, it is time we made use of the SugarCRM server that we deployed in the previous chapter.

In this chapter, we will take a guided tour of SugarCRM. Our task-oriented, systematic treatment will also relate the specifics of the SugarCRM system and its user interface with the general principles of CRM systems.

This chapter will cover the basic CRM functions and show how they are interpreted and performed within SugarCRM. These basic CRM concepts include the following:

- Common CRM processes and terminology
- Creating accounts and contacts, and relating multiple contacts to a single account
- Following links between related data to get a full view of a customer
- Creating and tracking the Sales Pipeline from leads to opportunities to contacts and accounts
- Creating and monitoring sales activities and accumulating activity history
- Scheduling activities with colleagues by referring to their calendars

### CRM processes and terminology

A CRM tool is a system that first needs to be taught all that there is to know in terms of the static information about your accounts and the contacts you have at those accounts. Once the CRM gains that knowledge, it leverages that knowledge by retaining and organizing information about all your daily business activities relating to those accounts and contacts. After a few weeks of regular use, the CRM system will be capable of easily presenting valuable historical information relating to your accounts and contacts, including successful and unsuccessful sales opportunities, interactions with your staff, or their customer service inquiries.

## Accessing the SugarCRM system

Before you start using the system, ensure that you have the appropriate software installed and configured on your system. All you will need is the following:

- A current web browser running on your computer: SugarCRM has been tested with and supports a variety of browsers. The following browsers are known to work with SugarCRM:
  - Firefox version 2.0 and higher (www.mozilla.org/firefox)
  - Apple Safari version 3.0 and higher (www.apple.com/safari/)
  - Microsoft Internet Explorer version 6 and higher (www.microsoft.com/ie)

You may encounter problems if you try to access SugarCRM using older web browsers like Internet Explorer 4 or Netscape 4.x. If you are unsure about which web browser version you are using, click on Help | About, or similar options on the menu bar in your browser. The version number will be displayed.

- JavaScript and cookies support enabled in your web browser: Both JavaScript and cookies support must be enabled in the security settings of your browser and are usually turned on by default. If you encounter problems accessing the system, check your browser configuration settings to ensure both JavaScript and cookies support are enabled. (See Tools | Internet Options | Privacy and Security tabs in Internet Explorer, or Tools | Options | Privacy and Web Features tabs in Firefox.)

- Network access to a server that is running the SugarCRM software: Your system or network administrator will be able to provide you with an Internet address (URL) at which the system can be accessed. If you do not already have an access to an instance of SugarCRM, refer to Appendix A and B for instructions on installing of Linux and Windows respectively.

Next, we need to log in to the system and choose a language and a theme (which determines how the application will look). If you do not provide the correct username and password, SugarCRM will not give you an access to the system.

Using the admin area within the system, your System Administrator assigns login credentials—a username and password—to every system user. This admin area is only accessible to users with administrative privileges. If you do not have a username and password combination, contact your system administrator.

To access SugarCRM, type the URL into your web browser’s address bar. You should see a screen similar to the one that follows. This is the SugarCRM login screen. If you do not see a login screen, verify that you have entered the URL correctly. If you did not make a typing mistake, contact your system administrator to verify that you have the correct URL.

Clicking on Options will allow you to select your desired Language, assuming languages other than English (US) have been installed. A Theme can also be selected from within the Options section. The choices you make are specific to your username and will not affect other users who also access the system.

The language selection directly impacts your ability to interact with the system by providing an interface written in a language with which you are comfortable. Themes are used to make visual changes, such as colors and graphics, and are more a matter of personal taste. Note that if you are connecting to your SugarCRM system through a low bandwidth connection, you will be better off using a theme that utilizes fewer graphics. The following screenshot shows a SugarCRM login screen:

![](img/7ccaf60360d7c8e809d336edbba2263d_98_0.png)

Once the SugarCRM login screen appears, follow the following steps to log in:

1. Type your username in the User Name box
2. Type your password in the Password box
3. Select the Language and Theme that you want to use (if applicable)
4. Click on the Login button
