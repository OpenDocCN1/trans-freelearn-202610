
The action button 225

## Subscribing reports 226

Hands-on exercise for subscribing to a report in Lightning Experience 228

## Summary 230

## Chapter 9: Building Dashboards in Lightning Experience 231

## Introducing dashboards in Salesforce 232

Built-on components 232

Dashboard accessibility 232

Running user 233

Dynamic dashboards 233

The new experience 234

## Opening a dashboard 235

The Dashboards tab 236

The DASHBOARDS menu 238

The FOLDERS menu 240

The favorites icon 241

The search textbox 242

## Navigating to a dashboard 244

Dragging and dropping in the Dashboard Builder 245

## Refreshing dashboards 246

Scheduling a dashboard refresh 248

## Creating your first dashboard 253

Hands-on exercise for creating a dashboard 253

Hands-on exercise for editing and maintaining a dashboard 258

## The dashboard components 263

Defining the dashboard components 263

Tables 265

Funnel charts 266

Gauge charts 267

Stacked Bar charts 268

Scatter charts 269

Cumulative Line charts 270

Combination charts 270

Reports as a data source 271

## Summary 273

## Chapter 10: Learning Advanced Dashboard Configuration 274

## Implementing the dashboard filters 275

Getting to know the dashboard filters 275

Hands-on exercise for adding a filter to a dashboard 276

## Viewing a dashboard 284

Scheduling a Reporting Snapshot ........................................ 343
Summary and schedule .................................................... 345
History and monitoring a Reporting Snapshot ............................. 347
Reporting on historical data ............................................ 348

## Summary ................................................................ 349

## Chapter 12: Dashboards and Reports in the Salesforce1 Mobile App .... 350

## Introduction to the Salesforce1 mobile app .......................... 351

## Working with dashboards in the Salesforce1 app ...................... 352

Refreshing a dashboard ................................................. 355
The dynamic dashboard .................................................... 356
The dashboard filters ..................................................... 357
Sharing a dashboard snapshot to Chatter feed ............................. 358
Changing the component chart types ....................................... 360
Chart details and reports ................................................ 361

## Working with reports in the Salesforce1 app .......................... 363

Report menu items ....................................................... 363
Drilling down from dashboards ............................................ 364
Drilling down to records .................................................. 366

## Accessing offline ................................................... 366
Limitations of the Salesforce1 app .................................. 366
Summary ................................................................ 367

## Index .................................................................. 368

## Preface

This book is for Salesforce administrators, business users, and managers who use Salesforce Lightning Experience in their daily work or to analyze data in Salesforce. It covers all items related to reporting and dashboards in Salesforce Lightning Experience. This book will benefit business users, who will gain knowledge ranging from creating basic reports to advanced report and dashboard configuration. Administrators will learn the entire concept of reporting and dashboards, object models, permissions related to reports and dashboards, report storage, and reporting on historical data.

Reports and dashboards are among the most powerful and easy to use features in the Salesforce platform, including Sales Cloud, Service Cloud, and Force.com, where business users are able to create and customize reports and dashboards as needed in minutes.

Business users should have basic knowledge, or should be using Salesforce in their daily work, such as logging in to the system, navigating through the Salesforce Lightning environment, creating and editing data, and running reports in Salesforce. Admin users should have basic knowledge of customizing Salesforce, such as the Setup menu, tabs, user profiles, permission sets, and standard and custom objects.

### What this book covers

Chapter 1, *Fundamentals of Salesforce Reports and Dashboards*, provides a basic overview of the Salesforce Lightning Experience, object model, Setup menu navigation, and navigating reports and dashboards.

Chapter 2, *Concepts and Permissions in Reports and Dashboards*, talks about the relationship between data visibility and result in a report generated, continues about report and dashboard storages, and permissions for reports and dashboards.

Chapter 3, *Implementing Security in Reports and Dashboards*, discusses how to secure and limit accessibility to the reports and dashboards stored in Salesforce.

Chapter 4, *Creating and Managing Reports*, covers the basic skills in required to create a new report, ranging from selecting a report format, adding filters to the report, and hands-on creation of simple report.

Chapter 5, *Understanding Report Types*, explains the report type as the foundation of the report which will also determine the availability of objects and fields, including data in the generated report. This chapter share about the standard report types, including creating custom report types.

Chapter 6, *Advanced Report Configuration*, discusses categorizing data in reporting using bucket fields and custom summary formulas.

Chapter 7, *Adding Charts in Reports and Pages*, talks about the types of charts available in the report for Lighting Experience, adding a chart to the report, and also using the same chart on the record page.

Chapter 8, *Working with Reports*, covers working with reports and items available under the Reports tab, the report folder, and things that can be configured after running a report.

Chapter 9, *Building Dashboards in Lightning Experience*, covers opening and searching for dashboards, and navigating, building, and refreshing dashboards and the components in them.

Chapter 10, *Learning Advanced Dashboard Configuration*, explains the advanced functionalities in dashboards, including dashboard filters, dynamic dashboards, drilling down, and getting additional dashboards from AppExchange.

Chapter 11, *Advanced Tips and Tricks for Reports and Dashboards*, covers collaboration in reports and dashboards, filtering reports using URL parameters, Field History Tracking, and setting up Reporting Snapshot.

Chapter 12, *Dashboards and Reports in the Salesforce1 Mobile App*, shows you how to access reports and dashboards on the go, using the Salesforce1 mobile app, including offline access without the internet.

## What you need for this book

The best approach to get the most of this book is to get hands-on experience of all the exercises. You will need the following:

- A Salesforce account. You are advised to use the Enterprise edition or above. However, for the purposes of testing and learning, a Sandbox instance and Developer edition are good enough.
- A web browser.
- An internet connection.
- A computer.
- A mobile phone--iOS or Android.

## Who this book is for

This book is written for business analysts, reporting analysts, sales representatives, sales operations, and Salesforce administrators. This book is also for users who want to learn about Salesforce reporting and dashboards in depth using Lightning Experience. You will gain knowledge of items related to reports and dashboards in Salesforce, starting from creating basic reports and customizing them, and moving up to the most advanced reports and dashboards in Salesforce Lightning Experience.

## Conventions

In this book, you will find a number of text styles that distinguish between different kinds of information. Here are some examples of these styles and an explanation of their meaning.

Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: "Create an Account report and use the No of Won Opportunity field as the report filter."

Any command-line input or output is written as follows:

```
SELECT Id FROM Report USING SCOPE allPrivate WHERE OwnerId =
'00541000001cTYgAAM'
```

New terms and important words are shown in bold. Words that you see on the screen, for example, in menus or dialog boxes, appear in the text like this: "Navigate to the Reports tab and click New Report button."

![](img/72d01bea5a9ccf02f9452aab7505d9de_20_0.png)

Warnings or important notes appear like this.

![](img/72d01bea5a9ccf02f9452aab7505d9de_20_1.png)

Tips and tricks appear like this.
