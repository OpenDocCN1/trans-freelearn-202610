
Figure 25.2 will give you a quick idea about which methodology should be adopted when and under what circumstances. Also, you will understand some of the advantages and disadvantages of each of these methodologies so that you can take a better decision for your custom applications.

Note that this table should not be treated as a comprehensive list. For a detailed evaluation of the multitude of deployment approaches, kindly go through the Documentum manuals and white papers.

| Entity to be migrated | Methodology | Advantages | Disadvantages |
|---|---|---|---|
| Docbase objects | DocApp migration | Useful in selectively migrating objects in a Docbase. | Requires setting up installation options meticulously to avoid target Docbase from being corrupted/damaged. |
| Docbase objects | Dump & Load scripts | Useful in migrating an entire Docbase. | Time-consuming process if the Docbase size is huge. |
| Docbase objects | Custom-written scripts | Useful if the applications cannot be migrated by the available methodologies. | Requires strong scripting knowledge and understanding of Documentum architecture/objects. |
| Docbase content | DocApp migration | Useful in selectively migrating contents in a Docbase. | Workflow states/tasks for contents are migrated incorrectly on target Docbase. Also the creation and modification dates of objects in source Docbase are lost on the target Docbase. |
| Docbase content | Dump & Load scripts | Useful in migrating an entire Docbase. | Time-consuming process if the Docbase size is huge. |
| Docbase content | Custom-written scripts | Useful if the applications cannot be migrated by the available methodologies. | Requires a strong scripting knowledge and understanding of Documentum architecture/objects. |
| Docbase content | Documentum FTP Services | Useful for migrating bulk content into a Docbase. | Requires manual drag and drop to import files into Docbase. |
| Web Publisher code and customizations/ processing scripts | Manual deployment | None. | Error-prone since requires manual intervention and time-consuming as well. |
| Web Publisher code and customizations/ processing scripts | Automated build scripts using ANT | Systematic means to compile and package code. | Requires good knowledge of ANT scripting. |

Figure 25.2: Deployment approaches

In this chapter we will discuss DocApp migration as a methodology to selectively migrate Docbase objects and content, and ANT scripts to migrate Web Publisher code and custom scripts.

## 25.1 DocApp Migration

Using Documentum Application Builder you can create and package all your objects within a DocApp (refer to Chapter 2). In our case, we created a DocApp with the name TestDocApp to include our custom Documentum objects. Once all the objects have been packaged within a DocApp, you can create a DocApp archive using Documentum Application Builder. A DocApp archive is nothing but a ZIP of the included objects and their related objects in a proprietary format that can be read by Documentum Application Installer and released on a target Docbase. However, migration using Application Builder and Application Installer is not as easy as it sounds. There are a couple of things you need to keep in mind to ensure smooth migration. The prerequisites mentioned below list some basic points that you need to take care of if you are using DocApps for migrating Docbase objects and content across environments.

The installation options need to be set correctly for all objects that need to be archived and installed on the target Docbase. We will be covering this in detail later in this chapter.

Be sure to include all objects in your DocApp that need to be migrated onto the target Docbase. Documentum does not automatically include related objects. For example, if you include a Web Publisher Template file in the DocApp, Documentum does not automatically include its associated Rules and Presentation files in the DocApp.

Remove any references to objects that have been deleted in the source Docbase while selecting a DocApp archive. For example, if an Alias Set references a particular group name and this group no longer exists in the source Docbase, then remove its reference from the Alias Set before creating a DocApp archive.

If you have some of the objects already available on the target Docbase and you need to incrementally update them, make sure you do not change their names (object_name property) on the source Docbase while creating a DocApp archive. This is because DocApp archiving and installation looks at the object_name property to locate and identify any existing objects in the target Docbase.

Let us now go through some simple steps to include objects in a DocApp and to set their installation options in the source Docbase. We will then take an archive of this DocApp and install the archive on a target Docbase using Documentum Application Installer.

## 25.2 Configuring DocApp Objects and Creating/ Installing a DocApp Archive

This section will cover in detail the steps for setting installation options for DocApp objects and explain how to create and install a DocApp archive.

- 1. Check-out the DocApp and select menu option Insert | Object from Docbase | Object Type (say) to include an object type in your DocApp. Correspondingly, there are options available in Application Builder to insert other objects such as workflow templates, alias sets, etc. as well.

All object types are stored in the system cabinet corresponding to the name of the Docbase (dev_doc in our case). Select the object type to be included in the DocApp and click the Insert button (refer to figure 25.3).

![](img/b18244e6621ecc39cef03ff6c0e55e7c_415_0.png)

Figure 25.3: Inserting an object type from Docbase into a DocApp

Include all other objects that are to be deployed to the target Docbase via the menu option Insert | Object from the Docbase as depicted in figure 25.4:

| Object | Menu option |
|---|---|
| Object Types | Object Type |
| Document Lifecycles | Document Lifecycle |
| Workflow Templates | Workflow Template |
| Permission Set Templates | Permission Set Template |
| Alias Sets | Alias Set |
| Formats | Format |
| XML Applications | XML Application |
| **Data Objects** |  |
| Custom Folder (Custom_Fld) | Folder |
| Presentation Files folder (Custom_Presentations) | Folder (If Presentation files need to be included selectively, choose menu option Document.) |
| Rules Files folder (Custom_Rules) | Folder (If Rules files need to be included selectively, choose menu option Document.) |
| Template Files folder (News_Articles) | Folder (If Template files need to be included selectively, choose menu option Document.) |
| Folder Map | Document |
| Web Cabinet (Test_WebCabinet) | Cabinet |

Figure 25.4: Menu options for including objects in DocApp

After you have included all objects that need to be migrated, the left-hand pane of the Application Builder will appear as shown in figure 25.5:

![](img/b18244e6621ecc39cef03ff6c0e55e7c_416_0.png)

Figure 25.5: Included objects in custom DocApp TestDocApp

Note that we have included default Web Publisher object types such as wcm_category, wcm_channel, and wcm_channel_fld since we need to migrate template categories, content, and web cabinet (channel) folders from the source Docbase to the target Docbase.

### Documentum 5.3 Update

Documentum release 5.3 has introduced a new feature called **Modules** in Documentum Application Builder. Modules are nothing but elements of executable code that are represented in the Docbase repository as a `dmc_module` folder object type. For example, Business Objects in Documentum can be treated as a kind of module. This allows you to package your business object classes and supporting files (in the form of JAR files) in a DocApp that can be installed via Documentum Application Installer on a target environment.

- 2. After you have included the objects in the DocApp, select individual objects in the left-hand pane and choose menu option DocApp | Set Installation Options to set their installation options.

Setting installation options is extremely important because by doing so you provide a lot of vital instructions to the Application Installer for installing objects in the target Docbase such as:

- **Upgrade options**: If DocApp Installer finds an object with the same name as in your source Docbase, it can be directed to perform one of the following options:
  - Overwrite the corresponding object in the target Docbase with the version existing in the source Docbase.
  - Update and create a new version of the object in the target Docbase, leaving the original version(s) intact.
  - Do not overwrite the object in the target Docbase.
- **Data object transfer**: This option is available in the case of XML Applications, Folders, and Cabinets and it directs the installer to include or exclude objects in the manner described as follows:
  - **All content**: Installer will install the specified cabinets, folders, and sub-folders and all objects contained within them.
  - **Top level content**: Installer will install the specified XML Application folder or folder and the objects contained within it.
  - **Hierarchical structure**: Installer will install only the specified cabinets, folders, and sub-folders but no objects.
  - **Object only**: Installer will install only the specified XML Application folder, cabinet, or folder.

Let us take an example of custom lifecycle object `Custom_Lifecycle` and set its installation options.
