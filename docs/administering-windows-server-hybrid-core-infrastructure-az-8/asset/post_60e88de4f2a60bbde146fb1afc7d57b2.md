
Let’s move on to the exercise.

## Exercise – installing AD DS on Windows Server

This section will teach you how to install AD DS on Windows Server.

The following steps must be carried out on the local OS of a machine you have admin access to. We will install the AD DS role directly on the server we wish to be our first DC in our *new domain*, in a *new forest*; we will not use remoting.

The Server Manager *Add Roles* wizard and the *AD DS Configuration* wizard are used to install and configure AD DS.

Follow these steps:

**Note**
The `dcpromo.exe` AD DS Installation Wizard has been deprecated as a deployment method starting with Windows Server 2012.

- 1. Log in to your server. Then, from **Manage** in Server Manager, click **Add Roles and Features**:

![](img/cf35f46a8dd5f11e58349c1d81611b75_57_0.png)

Figure 1.15 – Server Manager

- 2. On the **Before you begin** page, click **Next**.

- 3. On the **Select installation type** page, leave **Role-based or feature-based installation** set and click **Next**.

- 4. On the **Select a destination server** page, leave **Select a server from the server pool** set and ensure the server where you want to install AD DS is selected. Then, click **Next**.

- 5. From the list of available roles on the **Select server roles** page, select the box for the **Active Directory Domain Services** role:

![](img/cf35f46a8dd5f11e58349c1d81611b75_58_0.png)

Figure 1.16 – The Select server roles screen

- 6. From the **Add Roles and Features Wizard** pop-up screen, click **Add Features**:

![](img/cf35f46a8dd5f11e58349c1d81611b75_58_1.png)

Figure 1.17 – The Add Roles and Feature Wizard pop-up screen

- 7. Back on the **Select server roles** page, click **Next**.

- 8. On the **Features** page, leave all the defaults as-is, review the features selected as a reference, and click **Next**.

- 9. On the **AD DS** page, review the information and click **Next**.

- 10. If required, select **Restart the destination server automatically** on the Confirmation page. Then, review the selections installed as a reference and click **Install**.

- 11. On the **Results** page, observe and monitor the installation progress; click **Close** when you see a message stating the **Installation succeeded on [YourServerName]:**

![](img/cf35f46a8dd5f11e58349c1d81611b75_59_0.png)

Figure 1.18 – The Installation progress screen

- 12. From Server Manager, click on Notifications, then Promote this server to a domain controller:

![](img/cf35f46a8dd5f11e58349c1d81611b75_60_0.png)

Figure 1.19 – The Promote this server to a domain controller notification screen

- 13. On the AD DS Configuration Wizard pop-up screen, note the deployment operation options on the Deployment Configuration screen. Select Add a new forest for this exercise:

![](img/cf35f46a8dd5f11e58349c1d81611b75_60_1.png)

Figure 1.20 – The Deployment Configuration screen

- 14. Enter a domain name under the Root domain name and click Next: For further reference, click the More about deployment configurations hyperlink at the bottom of the page:

![](img/cf35f46a8dd5f11e58349c1d81611b75_61_0.png)

Figure 1.21 – The Deployment Configuration screen

- 15. On the Domain Controller Options page, note that the Domain Name System (DNS) server and Global Catalog (GC) options are selected by default but that the Read-only domain controller (RODC) option is not available; leave all the defaults as-is and enter the DSRM password and confirm. Then, click Install.

For further reference, click the More about domain controller options hyperlink at the bottom of the page:

![](img/cf35f46a8dd5f11e58349c1d81611b75_62_0.png)

Figure 1.22 – The Domain Controller Options screen

- 16. On the **DNS Options** page, ignore the message that states that delegation for this DNS server cannot be created and click **Next**.

- 17. Wait while the NETBIOS domain name is auto-populated on the Additional Options page. Then, click **Next**:

![](img/cf35f46a8dd5f11e58349c1d81611b75_62_1.png)

Figure 1.23 – The Additional Options screen

- 18. From the **Paths** page, specify the location for the AD DS database, log files, and SYSVOL. For this exercise, leave the defaults as-is. Then, click **Next**.

For further reference, click the **More about Active Directory paths** hyperlink at the bottom of the page:

![](img/cf35f46a8dd5f11e58349c1d81611b75_63_0.png)

Figure 1.24 – The Paths screen

- 19. From the **Review Options** page, review the selections and click **Next**.

- 20. The **Prerequisites Check** page confirms the status of all prerequisite checks. A green tick should appear with a message stating **All prerequisite checks passed successfully**. Click ‘**Install**’ to begin the installation. Then, click Install:

![](img/cf35f46a8dd5f11e58349c1d81611b75_64_0.png)

Figure 1.25 – The Prerequisites Check screen

- 21. From the **Installation** page, observe and monitor the installation progress; your server will automatically restart:

![](img/cf35f46a8dd5f11e58349c1d81611b75_65_0.png)

Figure 1.26 – Installation progress screen

- 22. From the login screen of the server, you will need to use the domain account for your user, not the local account; this will be in UPN format – that is, UserName@domain.name:

![](img/cf35f46a8dd5f11e58349c1d81611b75_65_1.png)

Figure 1.27 – Server login screen

- 23. From **Server Manager**, you will see that the AD DS role has been installed, as well as the DNS:

![](img/cf35f46a8dd5f11e58349c1d81611b75_66_0.png)

Figure 1.28 – Server Manager

- 24. From Server Manager, click Tools, then Active Directory Administrative Center:

![](img/cf35f46a8dd5f11e58349c1d81611b75_67_0.png)

Figure 1.29 – AD Administrative Center

Congratulations! You have completed this exercise and installed AD DS on Windows Server.

In this exercise, you installed AD DS on Windows Server and accessed it via the ADAC. This helped you reinforce this chapter's theory, along with some practical skills.

Now, let's summarize this chapter.

## Summary

This chapter has provided coverage for the AZ-800 Administering Windows Server Hybrid Core Infrastructure exam learning objective: Deploy and manage AD DS in on-premises and cloud environments.

We learned about AD concepts, its services, such as AD DS and its components. We looked at the creation and management of AD DS instances in on-premises environments. A hands-on exercise then finished the chapter to provide you with additional practical skills.

This chapter aimed to take your knowledge beyond the exam objectives; we added new skills and learning with the content provided. This further develops your knowledge and skills for on-premises network infrastructure services and enables you to be prepared for a real-world, day-to-day hybrid environment-focused role.

In the next chapter, you will learn about some of the concepts and components of Azure AD DS and learn how to create and manage an Azure AD.

## Further reading

This section provides links to additional exam information and study references:

- Microsoft Certified: Windows Server Hybrid Administrator Associate: https://docs.microsoft.com/en-us/learn/certifications/windows-server-hybrid-administrator/
- Exam AZ-800: Administering Windows Server Hybrid Core Infrastructure: https://docs.microsoft.com/en-us/learn/certifications/exams/az-800
- Exam AZ-800: skills outline: https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWKI0r
- Microsoft Learn: Deploy and manage identity infrastructure: https://docs.microsoft.com/en-us/learn/paths/deploy-manage-identity-infrastructure/

## Skills check

Challenge yourself regarding what you have learned in this chapter:

- 1. What is Active Directory?
- 2. List three services that form part of Active Directory.
- 3. What is Active Directory Domain Services?
- 4. Explain the difference between AD and Azure AD.
- 5. What is Hybrid Identity?
- 6. What is the difference between Authentication and Authorization?
- 7. List seven logical components of AD DS.
- 8. List six physical components of AD DS.
- 9. What are AD DS objects?
- 10. What is an OU?
- 11. What is a service object, and how does it differ from a user object?
- 12. What is the difference between Standard and Group Managed Service Accounts?
- 13. What is a UPN?
- 14. Explain the group types and scopes.
- 15. Explain domains and forests.
- 16. List the trust types.
- 17. Explain trust directions.
