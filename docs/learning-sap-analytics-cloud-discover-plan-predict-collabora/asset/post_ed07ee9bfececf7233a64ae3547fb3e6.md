
## DataSource and Models
### DataSource
- On-Premise Applications
- Live Data
- Import Data
- Cloud Apps
- Files
### Models
- Analytics Model
- Planning Model
- Measures
- Dimensions
### Visualization
### Stories
- Pages
- Canvas
- Responsive
- Grid
- Charts
- Geo Map
- Table
- Static & Dynamic Text
- Images & Shapes
### Collaboration
- Events & Tasks
- Comments
- Create PDF
- Share Stories
- Discussions
- Share Files
### Presentation
- Create Agenda
- Create Agenda Sub Item
- Start Online Meeting
- Invite People
### Administration & Security
- Set up Users & Teams
- Set up Roles and Assign Privileges
- System Monitoring
- Deployment

## The Home screen
When you successfully log in to SAP Analytics Cloud, you are greeted with the Home screen.
Note that the Home screen is private to every SAP Analytics Cloud user, so it can't be shared.
Here are a few pointers to help you get acclimatized to the Home screen:

![](img/c69568baeee565fe02397867c05883e1_33_0.png)

- 1. To remove the Get Started widget, the Recent Stories tile, or the Welcome message, go to the Home menu, and deselect the item you want to remove.
- 2. The Home screen also allows you to add useful reminders for upcoming events and personal notes.
- 3. Using the Edit Background option, you can customize the display background and add a logo to the Home screen.
- 4. The first tile titled Recent Stories lists the five most recent stories you have accessed.
- 5. Your recent stories and visualizations are displayed as tiles. You can pin additional tiles from your stories to the Home screen.
- 6. Click the ellipsis icon to modify or delete a note.
- 7. The toolbar appears on top of every screen. Using the icons on the toolbar, users can see their own profiles, view notifications, get online help, view the time panel, and collaborate.

## The main menu
The following screenshot and bullets will help you understand the main menu better:

![](img/c69568baeee565fe02397867c05883e1_34_0.png)

- 8. The main menu icon is your gateway to SAP Analytics Cloud. It provides you with all the options to get things done.
- 9. Click on Create to add new Stories, Models, Process, Dimension, Currency, and Point of Interest. Models are discussed in Chapter 2, Models in SAP Analytics Cloud, and Chapter 3, Planning Model, whereas you will create stories in Chapters 4 to 7. The Allocation option under the Process sub-menu is discussed in Chapter 3, Planning Model.
- 10. Select Browse to access existing Stories, Models, Dimensions, Currencies, Point of Interest, and Files. Files are discussed in Chapter 8, Collaboration.
- 11. The Events option opens the Events page, where you can manage your planning and analytic activities and assign tasks to other users. This subject is discussed in Chapter 8, Collaboration.
- 12. The options provided in the Security menu items lets you create and manage application users, teams, roles, requests, and monitor data changes and user activities. These options are discussed in Chapter 10, System Administration.
- 13. Deployment includes creation and management of export and import of application content. This subject is discussed in Chapter 10, System Administration.
- 14. Using the Connections menu item, you can create and maintain connections to remote systems. You can create connections to import data into SAP Analytics Cloud or create live data connections to SAP HANA. You will see some instances of establishing connections later in this chapter.
- 15. The System menu item includes the Monitor, Administration, and Enabling SAML Single Sign-On (SSO) options. Using these options, the system administrator monitors the system, reads system events and errors in the system log, and sets up SAML Single Sign-On (SSO) to work with SAP Analytics Cloud.

Due to the frequent updates SAP makes to the application, the screenshots provided in this book might differ from yours.

## Data sources and models
Before commencing your analytical tasks in SAP Analytics Cloud, you need to create models. A model is the foundation of every analysis you create to evaluate the performance of your organization. It is a high-level design that exposes the analytic requirements of end users. Planning and analytics are the two types of models you can create in SAP Analytics Cloud.

Analytics models are simpler and more flexible, while planning models are full-featured models in which you work with planning features. Preconfigured with dimensions for time and categories, planning models support multicurrency and security features at both model and dimension levels. Chapters 2 and 3 of this book are dedicated to these two models.

To determine what content to include in your model, you must first identify the columns from the source data on which users need to query. The columns you need in your model reside in some sort of data source. SAP Analytics Cloud supports three types of data sources: files (such as CSV or Excel files) that usually reside on your computer, live data connections from a connected remote system, and cloud apps.

In addition to the files on your computer, you can use on-premise data sources, such as SAP Business Warehouse, SAP ERP, SAP Universe, SQL database, and more, to acquire data for your models. In the cloud, you can get data from apps such as Concur, Google Drive, SAP Business ByDesign, SAP Hybris Cloud, OData Services, and Success Factors. The following figure depicts these data sources. The cloud app data sources you can use with SAP Analytics Cloud are displayed above the firewall mark, while those in your local network are shown under the firewall.

As you can see in the following figure, there are over twenty data sources currently supported by SAP Analytics Cloud. The methods of connecting to these data sources also vary from each other. Due to space constraint, it is not possible to outline every connecting method in this chapter. However, some instances provided in this chapter would give you an idea on how connections are established to acquire data. The connection methods provided here relate to on-premise and cloud app data sources. Considering the audience of this book who may not have access to these data sources, and to keep things simple, Chapter 2, Models in SAP Analytics Cloud, will guide them to create an analytics model in the trial version of SAP Analytics Cloud using an Excel file provided with this book.

![](img/c69568baeee565fe02397867c05883e1_36_0.png)

> *Due to relevance with the topic, the sub-sections that follow are added here to provide an overview of different types of connections you can establish in SAP Analytics Cloud to build your models.*

![](img/c69568baeee565fe02397867c05883e1_37_0.png)

## Create a direct live connection to SAP HANA
Execute the following steps to connect to the on-premise SAP HANA system to use live data in SAP Analytics Cloud. Live data means that you can get up-to-the-minute data when you open a story in SAP Analytics Cloud. In this case, any changes made to the data in the source system are reflected immediately. Usually, there are two ways to establish a connection to a data source — use the Connection option from the main menu, or specify the data source during the process of creating a model. However, live data connections must be established via the Connection menu option prior to creating the corresponding model. Here are the steps:

- 1. From the main menu, select Connection.
- 2. On the Connections page, click on the Add Connection icon (+), and select Live Data Connection | SAP HANA.
- 3. In the New Live Connection dialog, enter a name for the connection (for example, HANA).
- 4. From the Connection Type drop-down list, select Direct. The Direct option is used when you connect to a data source that resides inside your corporate network. The Path option requires a reverse proxy to the HANA XS server. The SAP Cloud Platform and Cloud options in this list are used when you are connecting to SAP cloud environments. When you select the Direct option, the System Type is set to HANA and the protocol is set to HTTPS.
- 5. Enter the host name and port number in respective text boxes.
- 6. The Authentication Method list contains two options: User Name and Password and SAML Single Sign On. The SAML Single Sign On option requires that the SAP HANA system is already configured to use SAML authentication. If not, choose the User Name and Password option and enter these credentials in relevant boxes.
- 7. Click on OK to finish the process. A new connection will appear on the Connection page, which can now be used as a data source for models. The creation of the model is discussed in the next chapter. However, in order to complete this exercise, we will go through a short demo of this process here.
- 8. From the main menu, go to Create | Model.
- 9. On the New Model page, select Use a datasource.
- 10. From the list that appears on your right side, select Live Data connection.
- 11. In the dialog that is displayed, select the HANA connection you created in the previous steps from the System list.
- 12. From the Data Source list, select the HANA view you want to work with. The list of views may be very long, and a search feature is available to help you locate the source you are
