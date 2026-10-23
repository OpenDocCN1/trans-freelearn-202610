+   - **Azure AD Password Protection:** This is an Azure AD feature that can detect and block weak passwords within Active Directory. This uses the same global and banned password list that is used by Azure AD. This feature consists of an agent installed on your domain controllers that listens to password change events and determines whether new passwords are accepted or rejected:

![](bbox=[0.165, 0.215, 0.85, 0.545])

Figure 2.3 – Defining custom banned passwords within Azure AD

In addition, we have smart lockout, which determines how long a user should be locked out of Azure AD after X amount of failed login attempts.

+   - **Active Directory and Azure AD password policy:** Active Directory has a global default password policy within the default domain policy. This policy applies to all users within the domain. With Server 2008 and upward, you also had the option to create fine-grained password policies that can be defined for certain users or groups. It should be noted that the Azure AD password policy mechanism does not apply to users that are synchronized from on-premises unless we enable the **EnforceCloudPasswordPolicyForPasswordSyncedUsers** feature in Azure AD Connect.

+   - **Microsoft LAPS and CloudLAPS:** In many cases, you might need to have a local administrator account on machines in case of issues related to AD joined devices or Azure AD joined devices that might be failing, and you need to log in to troubleshoot. In those cases, many use the same local username and password on devices to make it easy to remember, which you should not do. Instead, use services such as Microsoft LAPS, which will automatically rotate passwords for