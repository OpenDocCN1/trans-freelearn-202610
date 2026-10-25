

## A quick tour of SugarCRM

After you have successfully logged into the system, the **Home** tab is displayed, as shown in the following screenshot. Note that this overview assumes that you have logged into the system using an account that has administrator level privileges. Various key elements of the screen layout are highlighted as follows:

![](img/7ccaf60360d7c8e809d336edbba2263d_99_0.png)

- **Navigation tabs**: By default, they are organized into functional groups. Clicking the main header allows you to choose a module, such as **Contacts** or **Accounts**.

- **System links**: **My Account**, **Employees**, **Logout**, and **About**. The **Admin** link is only available to users who are assigned administrative privileges.

- **Search box**: It helps to search for a text string within key data held by SugarCRM.

- **Last viewed**: It is a useful trail of recent records you have viewed.

- **Navigation shortcuts box**: These are specific shortcuts that are useful within each module.

- **Quick new item box**: It is a quick data-entry box to create a new item for the current module.

- **Main screen body**: On the **Home** tab, this includes **Cases**, **Opportunities**, **Appointments**, **Leads**, **Tasks**, and the sales pipeline graph. Additional items can also be added by using the **Add Sugar Dashlets** button.

The screen layout in the **Home** tab uses the same layout for all the other tabs. As you move between the tabs, the shortcuts available in the shortcuts list change according to a specific tab. The main screen body displays the information indicated by the _tab name_. The **Quick New Item** box changes to offer quick access for creating new items based on the tab, for example, **New Contact**, **New Account**, **New Opportunity**, and so on.

## Themes

Earlier we mentioned that you can choose an alternative theme to change cosmetic aspects relating to your SugarCRM usage. Let us take a quick look at that before we move on. In the **System Links** area of the screen (top right), click on the **Page Styles** link and change the theme value to **Links**. Changing the theme value will cause SugarCRM to automatically apply the new settings.

Notice that many aspects of the SugarCRM screen will vary by changing the theme. The navigation tabs are no longer tabs, instead they are converted into a side menu system. Essentially, the shortcuts have switched positions with the tabs. In addition, the color scheme and graphics are very different.

Regardless of which theme you use, the same CRM information is displayed. It is just laid out differently. Pick the theme that you prefer, or change it now and then to keep some fun in your use of the CRM system. If you are interested in customizing an existing theme or perhaps creating your own to match your company's branding, you will find the necessary information in _Chapter 5_.

> For the rest of this chapter, we will assume that when you installed SugarCRM, you also opted to install the sample data. (Refer to a section about _Database Configuration_ within the appendices on installing SugarCRM for further information.) Later on, we will quickly re-do the installation to remove the demo data and then import your own organization's data.

## SugarCRM basics: Data relationships

Before we get too deep into specific functional areas of the application, it is important to understand certain aspects of the system's internal relationships and hierarchy.

Throughout this book we have referred to _Accounts_, _Contacts_, _Opportunities_, and other data entities that make up a CRM system. We have also discussed that some of the main objectives of a CRM system are to store this type of information and also easily demonstrate relationships among them. It is this philosophy that allows a CRM system to not only provide you with information about the sales that are pending, but also the clients they relate to. In order to accomplish this, rules dictating the manner in which data entities are interrelated, or can be connected, must exist within the CRM system. To better understand SugarCRM, we must first examine these rules that help build its hierarchy.

Within the world of SugarCRM, an Account is the top level entity. All other data items, including Contacts, Opportunities, Cases, and so on can all be easily linked to an Account by default. You are also permitted to link as many of those items as you like to any given Account. This functionality allows multiple contacts to be linked to a single account and is commonly referred to as a one-to-many relationship.

The following illustration depicts the hierarchy:

![](img/7ccaf60360d7c8e809d336edbba2263d_101_0.png)

While lower level data entities are easily linked to Accounts, they are limited in the manner in which they can be linked to each other. A Case, for example, expects to be linked to an Account by default. It can be associated with a Contact only after it is first linked to an Account. This example highlights an area that may require customization depending on whether your business uses a B2B or B2C model.

In addition, there are some relationships that can only be established in a single manner. A good example is that of a Task and an E-mail. It is possible to establish a relationship between the two, but in order to do so; you must access the e-mail in question and establish the relationship there because attempting to do the reverse is not allowed. Were you to try it by accessing the Task, you would be left with the impression that it was not possible to establish the relationship.

A final important point concerning relationships between the lower level items is that they often represent one-to-one relationships. The Task and E-mail example that was just explained highlights this quite well, as only one Task can be linked to any given E-mail.

## SugarCRM navigation: Accounts and contacts

So–here we are–SugarCRM!

Let us get a little more familiar with using the system. First, (and I will assume that you have switched back to the normal Sugar Theme from now on) try accessing a few of the modules, by first placing your mouse over the group header tab and then selecting one of the displayed module names. For example, hover over Marketing, and then choose **Accounts** to access the accounts module as shown in the following screenshot:

![](img/7ccaf60360d7c8e809d336edbba2263d_102_0.png)

Different sets of information are organized within different modules, and each module has a tab that you click on in order to access and use it.

The modules in SugarCRM Community Edition 5.5 are grouped in the following manner:

| Group header tab name | Modules |
|---|---|
| Home | Home, Dashboard |
| Sales | Accounts, Opportunities, Leads |
| Marketing | Campaigns, Contacts, Accounts, Leads |
| Support | Cases, Accounts, Contacts |
| Activities | Activities, Calendar, E-mails, Calls, Meetings, Tasks, Notes |
| Collaboration | E-mails, Documents |
| Reports | Dashboard |

The Professional and Enterprise Editions of SugarCRM expand on this list and some of the additional items provide modules for products, quotes, forecasts and reports, as well as team sales and workflow features.

The system administrator can control which modules are accessible to specific users, and in some cases, users can also hide or rearrange tabs. Users can also choose not to use groupings and instead use tabs that represent each module, depending on their navigation preference. If you are not using the group tabs approach, you will find that SugarCRM can only display 12 tabs across the navigation area. Additional modules are accessible through an overflow area available to the very right of the last tab that is visible. Using a theme that lists the modules on the left side like a menu system (such as Links or Pipeline) eliminates the need for the overflow area as all the modules accessible to the user are automatically displayed.

## List and detail view screens

Proceed to hover over Sales and click on the Accounts tab. This shows you a list of all accounts in the system. Screens that show a list in SugarCRM are called list views. This screen is the Accounts list view. If you have the demo data installed, your screen should look something like the following:

![](img/7ccaf60360d7c8e809d336edbba2263d_103_0.png)

Let’s imagine that we are looking at a list of RayDoc’s accounts—a sample set of accounts, contacts, leads, opportunities, and so on for the RayDoc organization. Clearly, the names of the companies are made up as we cannot expose information about real companies here, but the sample data here has been created to demonstrate all the key capabilities of the system that are needed to make your CRM deployment a real success.

