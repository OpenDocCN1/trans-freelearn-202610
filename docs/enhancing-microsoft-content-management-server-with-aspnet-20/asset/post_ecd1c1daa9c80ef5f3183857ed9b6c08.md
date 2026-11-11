
## Table of Contents
- Installing MCMS 2002 SP1a 45
- Remove Temporary Items 49
- Install Visual Studio 2005 49
- Install MCMS SP2 52
- Configuring the Development Environment 54
- Site Manager Shortcut 55
- Visual Studio Templates 55
- Run the Database Configuration Application 56
- Remove Visual Studio.NET 2003 64
- Known Issues with MCMS SP2 Installation 64
- Installation Tips 65
- Installing MCMS SP2 on Windows Server 2003 X64 Edition 65
- Installing the Required SQL Server 2005 Components 65
- Summary 66

## Chapter 2: Getting Started with the Development Environment 67
- Visual Web Developer Websites 67
- Creating an MCMS Web Application 68
- Checking the Website Configuration Settings in IIS 71
- Developing MCMS Web Applications 73
- Correcting Basic.aspx 74
- Configuring the 'CMS' Virtual Directory 76
- Creating Custom MCMS Application Templates 78
- Summary 87

## Chapter 3: Using Master Pages 89
- Overview and Benefits of Master Pages 89
- The TropicalGreen Website 91
- Importing the TropicalGreen Site Deployment Object File 92
- Creating a New MCMS Web Application 92
- Creating a Master Page for Use with MCMS 93
- Creating an MCMS Template File Based on a Master Page 98
- Modifying Master Page Properties from the Template File 102
- Summary 103

## Chapter 4: Navigation Controls 105
- Navigation for TropicalGreen 106
- Site Maps and Site-Map Providers 107
- Building an MCMS Site-Map Provider 108
- The MCMSSiteMapProvider Class 108
- Implementing the GetRootNodeCore() Method 110
- Implementing the FindSiteMapNode() Method 111
- Enhancing the Searches.GetByUrl() Method 112
- Implementing the GetParentNode() Method 113
- Implementing the GetChildNodes() Method 117
- Registering the SiteMapProvider 118

### The SiteMapPath Control

### The Menu Control
- Building a Horizontal Menu 119
- Using Custom Properties to Differentiate Between Top and Right Menu Items 121
- The MCMSSiteMapProviderTop Class 122
- Building a Dynamic Multi-Level Vertical Menu 125
- The MCMSSiteMapProviderRight Class 125

### The TreeView Control
- The Site Map Page 130
- Populating Nodes on Demand 134
- Removing the Leave Warning Message When Expanding Nodes in Edit Mode 136
- Getting the Populate-On-Demand Feature of the TreeView Control to Work in Channel-Rendering Scripts 139
- Using the TreeView Control in Summary Pages 142
- The SiteMapProviderSummary Class 143
- Configuring the TreeView Control to Display a Flat Listing of Items 145

### Summary

## Chapter 5: Applying Themes
### Themes versus Cascading Style Sheets
### Creating a Theme
- Applying a Theme to the Entire Site 151

### Skins for Server Controls
- Creating a Default Skin 152
- Creating a Name-Controlled Skin 154
- Completing the Skin 155

### Using Style Sheets
- Creating a Style Sheet 156
- Why Themes with Style Sheets May Not Work on MCMS Sites 158
- Applying Themes when the RenderBaseHref Property is Disabled 160
- Creating the CorrectThemes HTTP Module 161

### Summary

## Chapter 6: Authentication Controls and Membership Providers
### Authentication Controls
### Implementing an MCMS Membership Provider
### Using an MCMS Membership Provider
- Creating a Login Page Using the Login Control 175
- Using the LoginStatus and LoginName Controls 178
- Supporting Custom Authentication Schemes 181
- Summary 182

## Chapter 7: How-Tos, Tips, and Tricks 183
- Tip #1: How to Perform Cross-Page Postbacks 183
- Passing Data Between Pages 184
- The "Order a Plant" Wizard 185
- The Ordering Page 186
- The Checkout Screen 190
- Adding the "Order Plants" Menu Item 192
- Why Cross-Page Postbacks Do Not Work on MCMS Sites 193
- The CorrectCrossPostBack HTTP Module 196
- Tip #2: How to Implement an Ad Rotator 198
- The MCMSAdRotator 199
- Tip #3: Considerations for Microsoft Office SharePoint Server 2007 202
- Isolate Code that Uses the PAPI into Modules 203
- Use the Provider Model Design Pattern 206
- Summary 206
- Index 207

## Preface
Enhancing Microsoft Content Management Server with ASP.NET 2.0 delves into the integration of key ASP.NET 2.0 features such as Master Pages, Site Navigation, Themes and Skins, and the Membership Provider Model with Microsoft Content Management Server 2002 (MCMS) Service Pack 2.

Each chapter builds upon the last, walking through these new features available to MCMS developers and building a sample site similar to that presented in our previous book, Building Websites with Microsoft Content Management Server from Packt Publishing January 2005 (ISBN 1-904811-16-7).

## What This Book Covers
Chapter 1 walks you through the installation and configuration of MCMS 2002 Service Pack 2 (SP2), along with SQL Server 2005 and Visual Studio 2005 on a single developer workstation. There are two approaches to setting up a development environment for SP2: an upgrade from a previous SP1a installation or starting from scratch and building a fresh installation including SP2. We will cover both approaches in this chapter.

In Chapter 2 we spend some time getting familiar with the MCMS Service Pack 2 development environment, which is slightly different from what we are used to with previous versions of MCMS and Visual Studio. We will cover these changes and a number of tips for working within the SP2 development environment, such as the creation of custom MCMS Visual Studio 2005 templates.

Chapter 3 introduces you to one of the best new features introduced with ASP.NET 2.0, master pages, which allow developers to enforce common layout and behavior across pages within an application. While at first pass many master pages concepts are similar to those of MCMS templates, there are a number of benefits to be gained by taking advantage of master pages within MCMS applications. This chapter provides an overview of the benefits of using master pages and a step-by-step guide for implementing them in your MCMS applications, where they become master templates!

Chapter 4 covers the new ASP.NET 2.0 Navigation provider model and controls, and how to integrate them into your MCMS applications. Developing site navigation controls is an exercise that often leaves developers in a dilemma. Early versions of Visual Studio did not provide any ready-to-use navigation controls. Developers had to choose between spending many hours building controls from scratch or expand project budgets to purchase shrink-wrapped software.

In Chapter 5 we will see how a common look and feel can be applied efficiently to an MCMS site by using themes. We will create skins and cascading style sheets and demonstrate how they work together to define the appearance of a site. Finally, we will discuss an essential customization required for themes to work correctly in an MCMS site.

Chapter 6 covers the Membership Provider Model, one of the key new concepts introduced with ASP.NET 2.0, which makes it significantly easier to develop web applications that utilize third-party or custom membership stores. In addition ASP.NET 2.0 ships with a number of authentication controls related to role membership, which vastly reduce the amount of code required to implement forms authentication and associated functionality in your applications. This chapter shows how to use these features to improve the implementation of Forms Authentication and provides a more elegant solution for "account mapping" scenarios whereby authentication takes place against an external store and the accounts are mapped to Windows accounts for the purposes of MCMS authorization.

In Chapter 7 we present a number of tips along with code samples for working with ASP.NET 2.0 and MCMS SP2, and offer implementation advice for those considering migration to the upcoming Microsoft Office SharePoint Server 2007.

## What You Need for This Book
This book has been written for MCMS developers who are comfortable with the material presented in our previous book, Building Websites with Microsoft Content Management Server, and have a solid grasp of C#. To use this book you need to have access to the following:

- Visual Studio 2005 (any edition).
- Microsoft Content Management Server 2002 Service Pack 2 (any edition). A 120 day evaluation of MCMS is available from http://www.microsoft.com/cmserver.
