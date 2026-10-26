
## Share your thoughts

Once you’ve read *Microsoft Power BI Cookbook, Third Edition*, we’d love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we’re delivering excellent quality content.

## Download a free PDF copy of this book

Thanks for purchasing this book!

Do you like to read on the go but are unable to carry your print books everywhere?

Is your eBook purchase not compatible with the device of your choice?

Don't worry, now with every Packt book you get a DRM-free PDF version of that book at no cost.

Read anywhere, any place, on any device. Search, copy, and paste code from your favorite technical books directly into your application.

The perks don't stop there, you can get exclusive access to discounts, newsletters, and great free content in your inbox daily.

Follow these simple steps to get the benefits:

1. Scan the QR code or visit the link below:

   ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_30_0.png)

   https://download.packt.com/free-ebook/9781835464274

2. Submit your proof of purchase.

3. That's it! We'll send your free PDF and other benefits to your email directly.

## 1 Installing and Licensing Power BI Tools

Microsoft Power BI is a suite of business analytics tools and services that work together to access data sources, shape, analyze and visualize data, and share insights. Although not all the tools are required for every project or deployment of Power BI, synergies are available by utilizing the distinct features and use cases of multiple tools as part of integrated solutions, encompassing diverse data sources, rich data analytics, and visualization types.

In this chapter, we walk through the installation and configuration of the primary tools and services that **business intelligence (BI)** professionals utilize to design and develop Power BI content. This includes official Microsoft tools such as Power BI Desktop, Power BI Report Builder, SQL Server, the on-premises data gateway, and various third-party tools such as DAX Studio, ALM Toolkit, and Tabular Editor.

The recipes in this chapter are as follows:

- Installing and configuring Power BI Desktop
- Installing SQL Server and the AdventureWorks database
- Installing and using Power BI Report Builder
- Creating an on-premises data gateway
- Installing additional tools
- Thinking about licensing

### Technical Requirements

It is assumed that the reader has access to a Power BI Pro or **Premium Per User (PPU)** license, the rights to download and install the development tools on their machine, and the necessary access and rights to deploy and manage content in the Power BI service and utilize the Power BI mobile applications. In addition, the following are required to complete the recipes in this chapter:

- A Windows PC running Windows 10 or Windows 11
- At least 10 GB of disk space available; the Power BI Desktop download is up to 600 MB in size and the installed application is over 1.5 GB

Additionally, the Microsoft on-premises data gateway requires the following:

- A Microsoft account
- .NET Framework 4.7.2 (a gateway release from December 2020 or earlier)
- .NET Framework 4.8 (a gateway release from February 2021 or later)
- A 64-bit version of Windows 10 or later, or a 64-bit version of Windows Server 2012 R2 or later
- 4 GB of disk space for performance monitoring logs (in the default configuration)

### Installing and Configuring Power BI Desktop

Power BI Desktop is the primary tool used to develop the visual and analytical content that is then deployed and collaborated on in the Power BI service, embedded in other applications and portals, or even shared on the public internet. Although Power BI Desktop runs as a single application, it includes three tightly integrated components with their own options and settings:

- The Power Query and Power Query Editor experience, with its underlying M language and data mashup engine
- The Analysis Services tabular data modeling engine and its DAX analytical language
- The interactive reporting and visualization engine

Configuring and leveraging these capabilities, in addition to advanced analytics and customization features such as R, Python, mobile layout, and natural language queries, makes it possible to build robust and elegant BI and analytics solutions.

### Getting ready

Most organizations restrict the ability of employees to install software such as Power BI Desktop on their corporate devices. These organizations typically define a policy and process to push a particular version of Power BI Desktop to all user devices or certain groups of users, thus avoiding compatibility issues and ensuring users have access to recent features.

### How to install and configure Power BI Desktop

There are actually three different versions of the Power BI Desktop application, as follows:

- Power BI Desktop (Microsoft Store app)
- Power BI Desktop (Microsoft Installer)
- Power BI Desktop RS (Report Server edition)

The preferred method of installation is by using the Microsoft Store app. In order to install Power BI Desktop from the Microsoft Store, follow these steps:

1. Open the Microsoft Store app and search for Power BI Desktop. Alternatively, you can enter https://aka.ms/pbidesktop into any browser bar and the Microsoft Store app will automatically open to the Power BI Desktop app, as shown in Figure 1.1.

   It is important to make sure that you get the Power BI Desktop app and not just the Power BI app. The Power BI app is for viewing reports published to the Power BI service only.

   ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_33_0.png)

   *Figure 1.1: The Power BI Desktop app in the Microsoft Store*

2. Click the Get button.

3. Once the download and installation is complete, click the Open button.

Power BI Desktop is now installed, and you are ready to start leveraging this powerful tool for BI.

### How it works

Power BI Desktop supports a rich set of configuration options both for the currently open file and all instances of Power BI Desktop on the given device. These options control the availability of the preview and end user features and define default behaviors, resource usage, security, and privacy/isolation of data sources.

Regardless of the version of Power BI Desktop that is installed, these options are available by using the File menu in the ribbon, selecting Options and settings, and then Options, as shown in Figure 1.2.

*Figure 1.2: Power BI Desktop Options and settings*

Selecting Options brings up the Options window, displayed in Figure 1.3.

*Figure 1.3: Power BI Desktop Options (February 2021)*

**GLOBAL** options are applied to all Power BI Desktop files created or accessed by the user, while **CURRENT FILE** options must be defined for each Power BI Desktop file. The following steps are recommended for **GLOBAL** options:

1. On the **Data Load** tab, under **Data Cache Management Options**, confirm that the currently used data cache is below the **Maximum allowed (MB)** setting. If it is near the limit and local disk space is available, increase the value of **Maximum allowed (MB)**. Do not clear the cache unless local disk space is unavailable, as this will require additional, often unnecessary, queries to be executed.

   ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_35_0.png)

   *Figure 1.4: Global Data Load options*

   In addition, under the **Time intelligence** heading shown in Figure 1.4, consider turning off **Auto date/time for new files**. While it is perhaps convenient to have out-of-the-box calendar hierarchy functionality for date columns, this can also significantly increase the size of a data model, and it’s strongly recommended to utilize a common date dimension table, preferably from a corporate data warehouse source. Finally, under the **Type Detection** heading, it is recommended that you choose the **Never detect column types and headers for unstructured sources** option. The default detection of column types from unstructured sources, such as text or Excel files, will create a hardcoded dependency on the column names in the source file. Additionally, this default transformation will be applied prior to any filter expression and, thus, can require more time and resources to perform the refresh.

2. On the **Power Query Editor** tab, under the **Layout** header, ensure that **Display the Query Settings pane** and **Display the Formula Bar** are both checked, as seen in Figure 1.5.

   ![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_35_1.png)

   *Figure 1.5: Power Query Editor Layout settings*

