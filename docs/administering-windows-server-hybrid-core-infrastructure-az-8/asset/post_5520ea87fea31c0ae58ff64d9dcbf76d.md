
The settings applied to the user and computer objects will be the combined effect of policies linked at each level, creating a **Resultant Set of Policies (RSoP)**. There can be multiple GPOs linked at each container level.

When one GPO has the same policy setting as a different value to another linked GPO, there will be a conflict; for example, *prevent shutdown* may be *enabled* in a site GPO, but the same setting might have a value of *disabled* in a GPO linked to a lower container such as an OU GPO. By default, lower-level containers override the GPOs set at the higher levels; in this case, the result would be that a computer can be shut down, as the policy setting to prevent shutdown has been disabled through the order of precedence.

It is essential to consider that settings you intended to be applied by a GPO at one level are not *overridden* by another GPO processed afterward.

The default behavior, however, can be changed so that you can set **block inheritance** of policy settings on a domain or OU container. As its name suggests, this blocking action prevents the policy settings from GPOs linked at higher levels in the hierarchy from being applied.

In addition to blocking inheritance, one final control is the ability to *enforce* a linked GPO. This GPO’s settings will take precedence over any conflicting settings, maybe from a GPO linked at a higher level. An enforce overrides a block, meaning the settings will be enforced even if set to block inheritance.

The challenge with the Group Policy framework is that it can become a governance issue. Many GPOs can be created and linked to different containers. With inheritance, blocking, and enforcement, it can become very complex to understand the resulting or effective settings that would be applied to a user or computer; the use of inheritance blocking should be limited.

When we consider blocking and enforcement, this resulting precedence can be quite hard to track; the **Group Policy Inheritance** tab can simplify this, showing the resulting precedence of GPOs considering all factors, so you do not need to figure it out. This is shown in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_106_0.png)

Figure 3.8 – Group Policy inheritance precedence evaluation

This section looked at the GPO order of processing and inheritance. The following section will look at GPOs specific to Azure AD DS.

## GPOs in Azure AD DS

Azure AD DS manages Group Policy in much the same way as AD to manage settings for the user and computer objects, but with restrictions on what settings can be accessed and configured in the managed domain.

When Azure AD DS is implemented in an Azure AD tenant, two built-in GPOs are created for managing the configuration of users and computers. These are as follows:

- AADDC Users GPO
- AADDC Computers GPO

These are shown in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_107_0.png)

Figure 3.9 – Default Azure AD DS GPOs

You can create custom GPOs if you need additional settings not available in these GPOs. These can only be linked to OUs, and you can create additional custom OUs as required to meet your needs. For Group Policy administration privileges in the managed domain, you need to be a member of the AAD DC administrators group.

In a hybrid identity environment where both AD DS and Azure AD DS are utilized, group policies from the AD DS DCs will not synchronize to the Azure AD DS managed domain. You must manually create these settings in Azure AD DS to work around this. You should be aware that Azure AD DS only supports a flat GPO structure, so you may have to rethink how you will provide the same user and computer configuration if your existing GPO design does not translate directly into what is possible in Group Policy on Azure AD DS.

In this section, we defined GPOs in Azure AD DS. The following section will look at the tools to create and manage GPOs.

## GPO management and troubleshooting tools

GPOs can be managed primarily through a GUI. The GPMC is the built-in group policy management tool in the Windows OS. This has a built-in editor feature, **Group Policy Management Editor**. The GPMC can be launched with the `gpedit.msc` command.

This tool allows you to create, link, edit, and delete GPOs, provide reports, and model how they will affect users and the computers they are applied to.

The following command-line tools can also be used to manage and troubleshoot GPOs:

- GPMC
- GPRESULT
- GPUPDATE
- LDIFDE
- PowerShell

When you create a new GPO or edit a GPO, the GPO files are replicated to every member server every *90 minutes* (DC servers are replicated every 5 minutes). You don't need to have a user log off or reboot to apply the policy. You can also force an application if you don't want to wait by using the `gpupdate /force` command.

The **Group Policy Management Editor** pane in the following screenshot shows the configuration settings nodes of **Computer Configuration** and **User Configuration** in the GPO settings hierarchy and how they are visually represented:

![](img/cf35f46a8dd5f11e58349c1d81611b75_109_0.png)

Figure 3.10 – GPO configuration nodes

The Computer Configuration and User Configuration nodes in the GPO settings hierarchy are then broken down into sections with folders for Policies and Preferences, and beneath those are further configuration areas. The Group Policy Management Editor pane shows how the Policies and Preferences folders and subconfiguration group folders are visually represented in the following screenshot:

![](img/cf35f46a8dd5f11e58349c1d81611b75_109_1.png)

Figure 3.11 – GPO configuration settings

GPOs linked to an object container will have the following attributes to be aware of for managing GPOs, including the following:

- The link order
- The GPO name
- Whether the GPO is enforced or not
- Whether the link is enabled
- Whether the GPO is enabled
- Whether there is a WMI filter

The following screenshot shows the GPOs linked to a domain object container and its attributes:

![](img/cf35f46a8dd5f11e58349c1d81611b75_110_0.png)

Figure 3.12 – Linked GPOs

The Group Policy framework’s strong configuration capabilities mean it can manage nearly all configurable settings. The most granular level of the Group Policy framework is the individual policy setting. The policies can be one of three states:

- Not Configured
- Enabled
- Disabled

![](img/cf35f46a8dd5f11e58349c1d81611b75_111_0.png)

Figure 3.13 – Group Policy setting states

When we create a new policy, the default is that all settings are set to **Not Configured**. When a policy state is changed between **Enabled** and **Disabled**, the changes are applied to the computer and user objects affected by the GPO.

This section looked at the tools for creating and managing GPOs. In the next section, we will complete a hands-on exercise section to re-enforce some of the concepts covered in this chapter.

## Hands-on exercise

To support your learning with some practical skills, we will utilize the concepts and understanding gained from the earlier sections of this chapter and put them into practical application.

We will look at the following exercises:

- Exercise – Installing Group Policy Management tools
- Exercise – Implementing Group Policy

## Getting started

To get started with this section, if you do not already have an Azure subscription, you can create a free Azure account at https://Azure.microsoft.com/free. This free Azure account provides the following:

- 12 months of free services
- $200 credit to explore Azure for 30 days
- 25+ services that are always free

Let's move on to the first exercise for this chapter.

## Exercise – Installing Group Policy Management tools

This section will look at installing the Group Policy Management tools on a management server VM. This will allow you to create and configure GPOs and custom OUs in the managed domain.

For this exercise, we will use the Azure AD DS managed domain we created in the Azure AD tenant exercise in the previous chapter. If needed, please complete this exercise and then return. Alternatively, please use your own environment so that you can follow this exercise.

You will need the following additional requirements in place to complete this exercise:

- For this Azure AD DS managed domain, a user account that is a member of the **Azure AD DC administrators** group in the Azure AD tenant
- A Windows Server management VM joined to the Azure AD DS managed domain; if needed, please complete the steps in the following Microsoft article and then return: https://docs.microsoft.com/en-us/azure/active-directory-domain-services/join-windows-vm
