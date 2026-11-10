
In short, WDK is a set of server-side reusable components supported by Documentum that allow one to rapidly develop and deploy web-based applications.

WDK architecture consists of:

- **表示模型**: Incorporates JSP tag libraries to separate the UI from business logic.
- **组件模型**: Incorporates server-side components that extend the Content Server functionality.

Figure 3.2 represents a simplified version of the WDK architecture, showing how the various Documentum entities fit in the various layers.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_47_0.png)

Figure 3.2: Simplified WDK architecture

**Webtop** is a Documentum reference implementation with WDK components. Web Publisher tool is built over Webtop and is used mostly for content authoring and publishing.

WDK and its usage are discussed in detail in Chapter 24.

## 3.5 Web 发布器

Web Publisher is a browser-based web application provided by Documentum for content creation, review, and finally publishing to websites.

In the Documentum architecture, Web Publisher utilizes the content management services from Content Server, publishing services from **SCS** (Site Caching Services), and rendition (see section 3.9) management from **CRS** (Content Rendition Services).

Apart from these, Web Publisher also works with a host of other Documentum products.

Web Publisher defines its own specific groups in order to divide the responsibilities of the participating entities. For example, a content author can use Web Publisher to use predefined templates and create content for the website without requiring any knowledge of web design/construction.

A web developer on the other hand, understands web design and uses Web Publisher to create content templates, automates the review process through workflows, and maintains the website categories and structures.

An administrator has the highest privileges and maintains the Web Publisher system, administering and monitoring it and running reports for various objects and events.

Throughout this book, we shall explore Web Publisher in great detail and utilize it for various operations such as content authoring, categorizing, and publishing.

## 3.6 ACL

ACL (Access Control List) forms a part of Documentum's security mechanism to restrict certain groups/users from accessing a particular object. Documentum stores restricted objects as dm_acl objects. Note that all SysObjects in Docbase have an ACL assigned to them.

An ACL, also called a permission set, stores information regarding the groups/users that have access to a particular object as well as their access levels (or privilege levels).

Keep in mind that the permission levels are hierarchical, which means a permission level of 'READ' for example would grant permission levels above it as well—'NONE' and 'BROWSE'.

Go through the table shown in figure 3.3 to understand the basic access levels available in Documentum:

| 许可级别（r_accessor_permit 属性） | 权限 | 简短说明 |
|---|---|---|
| 1 | NONE | 对对象无访问权限。 |
| 2 | BROWSE | 用户可以查看对象的属性但不能查看其关联内容。 |
| 3 | READ | 用户可以查看内容但不能修改它。 |
| 4 | RELATE | 用户可以向对象附加注释（即评论）。 |
| 5 | VERSION | 用户可以创建对象的新版本但不能更新现有版本。 |
| 6 | WRITE | 用户可以写入并更新对象的现有版本。 |
| 7 | DELETE | 用户可以删除对象。 |

Figure 3.3: Basic permissions in Documentum

Apart from providing the above basic permission levels, Documentum provides some extended permission levels as well.

The table in figure 3.4 shows the extended permissions available in Documentum pre version 5.3:

| 权限 | 简短说明 |
|---|---|
| Change State | 用户可以更改对象的生命周期状态。 |
| Change Permission | 用户可以更改对象的权限。 |
| Change Ownership | 用户可以更改对象的所有者。 |
| Execute Procedure | 用户可以执行与对象关联的外部过程。 |
| Change Location | 用户可以更改对象的位置，例如从一个文件夹到另一个文件夹。 |

Figure 3.4: Extended permissions in Documentum (pre 5.3)

It is worth mentioning here that Documentum uses two reserved aliases in ACLs:

- `dm_owner`: Symbolizes the current owner of the object
- `dm_world`: This is an alias for all the users present in the Docbase

Let us take an example of a sample ACL attached to a folder in the Docbase:

**ACL 名称:** `OrganizationHR_ACL`

The permissions are as follows:

- `dm_owner`: DELETE
- `Hr_group`: WRITE
- `Finance_group`: READ
- `dm_world`: NONE

This means that the owner of the ACL (i.e. the one who created this ACL) has the highest rights—DELETE. The Human Resources (HR) group in the organization has WRITE access to the folder. The Finance group in the organization has just READ access to the folder. All other groups can not even see the folder since they have NONE access to the folder.

## 3.7 别名集

An Alias Set, as the name suggests is a list of Aliases. Now what exactly is an Alias? An Alias in plain terms should be considered to be a placeholder for value. An Alias resolves to a particular value called the Alias value. Alias sets are stored as dm_alias_set objects in Docbase.

Let us take an example here, in order to clear up all confusions.

An organization's workflow needs to go through a sequence of reviewers before it can be finally approved and published over to the website. A content author creates the content and needs to send it to his or her content manager for review. After the initial review, it needs to be finally approved by an approver.

Now, for each individual content author, his or her manager and approver would be different. Should a separate workflow be created individually for each content author? Or should it be hard-coded with the actual names of all the content authors in the organization and their content managers and approvers? What would happen if a new author or manager joins the organization in the future?

An alias set comes to our rescue! An alias set can be created for the workflow with placeholders for content authors, content managers, and approvers, etc. At run time, when the workflow is executed, the alias names can be substituted with their actual values.

In Documentum, an Alias Set can have placeholders for the following entities:

- Unknown (this is for the case when no existing entity fits)
- User
- Group
- User or Group
- Cabinet Path
- Folder Path
- Permission Set

Figure 3.5 shows a sample Alias Set in the Docbase as seen in Documentum Application Builder.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_50_0.png)

Figure 3.5: Sample alias set

We shall create custom Alias Sets in Chapter 12.

## 3.8 用户和组

It might sound bizarre, but Documentum stores users and groups in the form of objects!

### 3.8.1 用户

Users are stored as dm_user objects in the Docbase. We will now discuss some important attributes of the dm_user object:

- user_name: This is the actual name of the user and can contain spaces, for example 'Gaurav Kathuria'. Internally Content Server assigns this value to the owner_name attribute for the objects created by this particular user.
- user_os_name: This is what Documentum uses to validate the user's credentials for authentication purposes, for example 'gauravk'.
- user_address: This stores the user's email address and is used by Content Server to send notifications.

These three attributes are mandatory for a user object and should always be filled in when creating a Documentum user.

Each user has certain user privileges assigned to him or her:

- None (no special privileges)
- Create Type (can create object types)
- Create Cabinet (can create cabinets)
- Create Group (can create groups)
- Sysadmin (system administration privileges)
- Superuser (highest-level privileges)

Users in Documentum can be created through numerous mechanisms:

- DQL queries
- API commands
- LDIF file (import users)
- Documentum Administrator user interface
- DFC classes

### 3.8.2 组

Documentum groups are nothing but sets of Documentum users and groups. Documentum stores groups as dm_group objects. Let us take an example to understand the importance of groups in Docbase.
