
An Alias Set comes in handy in such circumstances, helping us design an application with symbolic names for content author/manager groups, the actual values for which are resolved by the system at run time as per the current context/environment.

You can specify multiple Alias Sets for a Lifecycle and have one of them designated as the default Alias Set; the others are treated as Additional Alias Sets. This is just the right time to discuss something called the scope of an Alias within Documentum. The scope of an Alias is the context in Documentum within which the system resolves the actual value for a given Alias name.

Documentum recognizes the following scopes:

- **Lifecycle scope:** An Alias Set for the Lifecycle. SysObjects in Documentum attached to the particular Lifecycle help the system resolve the values.
- **Workflow scope:** The Alias Set attached to the workflow process template is resolved at run time by finding the actual values of all performers in the workflow, whenever a workflow is started from the given workflow process definition.
- **Session scope:** The Alias Set for the life of a given Docbase session (session config object) helps the system resolve the actual value for an Alias.
- **User scope:** User scope is used to resolve Alias values from the Alias Set that was defined when the given users and groups were created within Documentum.
- **System scope:** This scope is used to resolve the values for Alias names for the Alias Set defined for the server config object for the given Docbase.

Whenever a content object is attached to a Lifecycle in Documentum, the system first evaluates all the Alias values based on the Alias Set attached to the Lifecycle object. This shows the importance of an Alias Set in the context of a Lifecycle. Hence, in this chapter, we will start by first studying how to create Alias Sets for a Lifecycle and then move on to build security in the various Lifecycle stages (using ACLs) and eventually create a full-fledged Lifecycle object.

## 12.2 Creating an Alias Set

In this section, we will discuss in detail the steps involved in creating an Alias Set.

In our custom DocApp TestDocApp, choose the menu option Insert | Alias Set to create a new Alias Set. Figure 12.1 shows an empty Alias Set screen. Provide a unique name for the custom Alias Set along with a short description to explain its purpose.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_174_0.png)

Figure 12.1: Blank Alias Set screen

To add a new Alias in the Alias Set, click on Add. Clicking this button opens up the Alias Object dialog box as shown in the figure 12.2.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_174_1.png)

Figure 12.2: Adding a new Alias in the Alias Set

Provide a name for the Alias in the Name text box. Choose an Alias Type from the Type dropdown. The available Alias Types in the dropdown are:

- Unknown
- User
- Group
- User or Group
- Cabinet Path
- Cabinet or Folder Path
- Permissions Set

Depending upon the chosen Alias Type, the values in Value field get populated when you click on the ellipsis button (...).

Choose the Prompt for value at installation checkbox if you wish the System to prompt for a value for the Alias while resolving the Alias.

Alias Category is not used by the system but can be utilized for categorizing the Alias Sets for your custom application.

A short description can be provided to explain the intent/purpose of the Alias. Click the OK button.

After you have provided Aliases for the Alias Set, the added Aliases are shown as individual rows in the Alias Set screen, as shown in figure 12.3.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_175_0.png)

Figure 12.3: Newly created Alias Set

Choose the Custom_AliasSet Alias Set shown in the left window pane of Documentum Application Builder, right-click, and choose Check in selected object(s) to check in the Alias Set in Docbase.

## 12.3 Creating Permission Sets

Security measures need to be in place so that during different stages of the Lifecycle, only certain authorized users working on the content objects get specific permissions or rights to work on the content. Say for example, our custom Lifecycle has four stages:

- Start (when content is being created)
- WIP (short for Work in progress)
- Staging (content under review)
- Approved (content finally reviewed and approved and ready to be shown on the live site)

You may design the ACLs (Access Control Lists or Permission Sets) for each of the Lifecycle stages. For example, during the Staging stage of the Lifecycle, the Administrators, Content Managers, Content Authors, and Web Developer users could have VERSION rights while the owner of the content could be given WRITE rights and the rest of the users (termed "world") could be given READ rights.

Please refer to our earlier discussion of ACLs in Chapter 3 to quickly understand the basic and extended permission levels in Documentum and their semantics.

Let us now create a Permission Set template (ACL) for the WIP Lifecycle state of the Lifecycle and accordingly create the ACLs for the other Lifecycle states.

1. In our custom DocApp TestDocApp, choose Insert | Permission Set Template. Provide some unique name (such as Custom WIP ACL) for the custom Permission Set along with some short description to explain its purpose (refer to figure 12.4).
2. Choose the first radio button Show Alias-Sets in Application to choose an Alias Set in our custom DocApp. Choose the second radio button Show All Alias-Sets in Docbase to choose an existing Alias Set from the existing Alias Sets in the Docbase. Choose the third radio button Show Users and Groups to directly select a group from all the existing groups in Docbase. Select an entry from the Alias names (if one out of the first two radio buttons is chosen) or select a group name (if the third radio button is chosen) and provide basic and extended permissions to each of the entities involved. Choose one of the seven basic permission levels from the Permissions dropdown and choose the checkboxes against the extended permission levels required (if any). There are in all five extended permission levels in Documentum. Accordingly you can click the Add to Set button to include the permissions in the ACL or click the Remove from Set button to remove the added permissions. Note that when you start creating a new Permission Set, Documentum automatically includes entries for dm_owner and dm_world aliases. You can obviously amend the specific rights for each of these two aliases.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_177_0.png)

Figure 12.4: Creating a Permission Set

3. After you have provided the necessary permissions, the Permission Set can be selected in the left window pane of Documentum Application Builder, right-clicked, and checked into the Docbase by selecting the Check in selected object(s) option.

Please note that we will not need to create an ACL for the Start stage of the Lifecycle since as soon as you create content in Web Publisher, Documentum automatically pushes it to the WIP Lifecycle state. In effect, the permission sets need to be created for all Lifecycle states beyond the Start state.

Figure 12.5 lays down a design of Permission Sets for each of the WIP, Staging, and Approved Lifecycle states based on Documentum's out-of-the-box ACLs for its Default Lifecycle. Needless to say, you may change the permission levels as per your business requirements.

| Lifecycle Stage | Alias | Basic Permission | Run Proc | Change Loc | Change State | Change Perm | Take Ownership |
|---|---|---|---|---|---|---|---|
| WIP | administrator | DELETE | Y | Y | Y | Y | N |
|  | content author | DELETE | Y | Y | Y | Y | N |
|  | content manager | DELETE | Y | Y | Y | Y | N |
|  | web developer | DELETE | Y | Y | Y | Y | N |
|  | dm_owner | DELETE | Y | Y | Y | Y | N |
|  | dm_world | READ | Y | Y | N | N | N |
| Staging | administrator | VERSION | Y | Y | Y | Y | N |
|  | content author | VERSION | Y | Y | Y | Y | N |
|  | content manager | VERSION | Y | Y | Y | Y | N |
|  | web developer | VERSION | Y | Y | Y | Y | N |
|  | dm_owner | WRITE | Y | Y | Y | Y | N |
|  | dm_world | READ | Y | Y | N | N | N |
| Approved | administrator | VERSION | Y | Y | Y | Y | N |
|  | content author | VERSION | Y | Y | Y | Y | N |
|  | content manager | VERSION | Y | Y | Y | Y | N |
|  | web developer | VERSION | Y | Y | Y | Y | N |
|  | dm_owner | WRITE | Y | Y | Y | Y | N |
|  | dm_world | READ | Y | Y | N | N | N |

Figure 12.5: Permissions for different Lifecycle states

## 12.4 Creating a Custom Lifecycle

In this section, we will discuss the detailed steps for creating a custom Lifecycle.

1. In our custom DocApp TestDocApp, choose menu option Insert | Document Lifecycle to create a new Lifecycle. Provide some unique name (such as `Custom_Lifecycle`) for the custom Lifecycle (refer to figure 12.6) along with some short description to explain its purpose.
