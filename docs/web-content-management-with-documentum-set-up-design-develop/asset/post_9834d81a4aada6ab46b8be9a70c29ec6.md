
Use character references and not HTML entity references when using special characters in Web Publisher XML files (Template files, Rules files, Presentation files).

For example, use &#163; to denote Pound symbol (£) instead of &pound;

This is due to the limitation of the Xerces XML parser, which Documentum internally uses to parse XML files.

- 3. Copy the template file created in the previous step to some location on your machine's local drive. Go to the newly created category (in step 1 above) and choose menu option File | Import. Click on Add Files (refer to figure 13.4) and browse your machine's local drives till you find the Template file to be imported. Then click on the Next button.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_207_0.png)

Figure 13.4: Importing Template file in Docbase

- 4. The Import: Object Definition screen is shown in figure 13.5. Provide some informative descriptive name for the template file in question and choose the object type with which it needs to be associated. Select the cust_newsarticle object type that we created earlier for our news article example.

After you select the object type, the Lifecycle dropdown gets populated with all the existing installed lifecycles in the Docbase that have the chosen object type as their acceptable primary/subtype.

Select Custom_Lifecycle from the Lifecycle dropdown and click the Import button.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_208_0.png)

Figure 13.5: Associating Template file with object type

- 5. The imported Template file is shown in the associated category in an Unavailable state as shown in figure 13.6. This signifies that even though it has been imported into the Docbase, it is not yet ready to be used by content creators.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_208_1.png)

Figure 13.6: Template file in Unavailable state

Run the following DQL query using the IDQL utility or through the DQL editor in Documentum Administrator:

```
DQL> select object_name, a_is_template, r_version_label, r_object_type
from cust_newsarticle where a_is_template = '1'
```

> Note that all content objects created from cust_newsarticle object type will have their a_is_template system attribute set to 0, while only the Template file will have this value set as 1.

### Result of the above DQL query:

The r_version_label attribute will have the value CURRENT, 1.0.

Now make the Template file available to content authors by choosing it and clicking the Make Available button shown in the tool bar (refer to figure 13.6).

Run the above DQL query again and note that now the r_version_label attribute of the Template file will have the value Approved, WIP, CURRENT, 1.0.

- 6. The Template file is now ready to be used by content authors, but it still needs to be associated with its supporting files such as Rules and Presentation files.

Choose the Template file and select View | Associations to look at the associated supporting files, as shown in figure 13.7.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_209_0.png)

Figure 13.7: Template file associations with supporting files

Congrats! You have successfully created a Template file in Documentum that can be used by content authors for entering content for their websites. Before we proceed any further and start associating the Template file with its supporting files, it is critical to first create the supporting files for the above Template file. Move over to the next chapter in order to learn how to create a Rules file for the Template file we created in this chapter.

## 13.2 Summary

We talked about two types of information for Documentum objects; the actual content that shows up on websites and the metadata or attributes for the content object.

In Web Publisher, content authors create content using Template files. Internally, Documentum creates content XML files based on the structure of Template XML file used for content creation.

A Template created in Web Publisher is attached to a Docbase object type. Whenever a template is used to create content, the associated objects are created as run-time instances of the linked object type.

As in Chapter 10, we again briefly discussed Web Publisher Rules and Presentation files. Finally, we looked at the detailed steps for creating a sample Template for our News Article example.

## 14 Creating Rules Files

Having created a Template file for our News Article example in the previous chapter, we are all set to go ahead and design a Rules file for the Template. Recall that a Rules file is a simple XML file used to define the various fields in a Template file. Rules files reside in Documentum Web Publisher within the Site Manager | Rules folder in the Docbase. Follow the steps mentioned in the following section to create a Rules file for your Template file. Note that Documentum allows you to associate a particular Rules file with multiple Template files.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_210_0.png)

Figure 14.1: Association of Rules file and template file

### 14.1 Creating a Rules File

In this section, we will discuss the detailed steps for creating a Rules file.

- 1. Log in to Web Publisher as an administrator or a web developer user and go to the Site Manager | Rules folder. It is a good approach to create a new folder within the Rules folder to identify and separate specific Rules files for your custom application.

Choose the menu option File | New | Folder. The new folder creation screen comes up as shown in figure 14.2.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_211_0.png)

Figure 14.2: New folder for storing Rules files

Provide a valid name for your custom Rules folder (say Custom_Rules) and click on the Finish button. You could also attach some ACL (Permission Set) to the custom Rules folder for security reasons, allowing and disallowing certain groups and users from accessing the same. This can be done by clicking on the Permissions tab shown in figure 14.2.

- 2. Rules files can be created using any of the available XML/text editors, but that would require you to understand and learn Documentum's Rules file syntax and follow the instructions as provided in Documentum manuals. Instead, Documentum has developed a simple Rules file editor application in Web Publisher that assists you in automatically creating a Rules file based on the parameters you choose/specify through a simple Rules file creation GUI screen.

A simple configuration step needs to be performed before you can start using the Rules file editor application. Click on the Preferences button in Web Publisher and go to the Web Developers tab as shown in figure 14.3.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_212_0.png)

Figure 14.3: Setting preferences for Documentum's Rules editor application

In the Rules Editor section, choose the radio button for Web-based application. Click on Apply and then OK. This ensures that when you create a new Rules file or edit an existing Rules file, Documentum's internal Rules file editor is automatically invoked.

- 3. Having set the Rules editor preferences, go within the custom Rules folder custom_Rules and choose menu option File | New | Rule. The new rule creation screen is shown in figure 14.4. Provide some valid name for the rules file (say cust_news_art_rule.xml) and click on OK.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_213_0.png)

Figure 14.4: Providing a name for the custom Rules file

- 4. The system asks you to browse through existing categories in the Docbase and choose the Template for which the Rules file needs to be created, as shown in the figure 14.5.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_213_1.png)

Figure 14.5: Existing categories in Docbase

Refer to figure 14.6; select the category that houses the template we created in the previous chapter (News_Articles) and select the checkbox against the Template file (cust_news_art_templ.xml in our case). Finally, click on OK.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_214_0.png)

Figure 14.6: Choosing the Template file for which a Rules file needs to be created

Documentum invokes the Rules file editor as shown in figure 14.7. We can now add Rules to the various fields in the Template file.

All the XML elements in the Template file are shown under the Tree View tab of the Rules editor. You simply need to select these XML elements individually and add Rules to them.

Note that within the Tree View tab, the template XML elements are in a read-only mode, preventing you from modifying the Template file structure. However, by switching to Text View tab, you have the flexibility to alter the structure of XML elements in the associated Template file.
