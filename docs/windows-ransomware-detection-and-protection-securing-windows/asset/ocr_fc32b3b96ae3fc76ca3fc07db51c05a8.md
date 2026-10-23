+   - We also have another feature in Active Directory called **time-based group membership**. This is a feature that is not enabled by default since it requires the forest function level to be 2016. To enable the feature, you will need to run the following command:

```
Enable-ADOptionalFeature 'Privileged Access Management Feature' -Scope ForestOrConfigurationSet -Target forest.com
```

Then, we can use the following command to temporarily add a member to a group for 15 minutes:

```
Add-ADGroupMember -Identity 'Domain Admins' -Members 'peter' -MemberTimeToLive (New-TimeSpan -Minutes 15)
```

This is also useful to ensure that we are using least privilege access rights.

+   - For Azure-based deployments, where we have virtual infrastructure running in Azure, we also have the option of using a feature called **Azure Bastion**. This allows us to log on to virtual machines using the Azure web portal. Azure Bastion supports remote management to Windows using RDP and Linux using SSH. It should be noted that Azure Bastion is a separate service and is not deployed by default.

+   - Ensure that administrators have a minimum of two accounts, where they have one regular user and another that is used for administrative purposes to manage the environment. This is to ensure that if the administrator account or endpoint gets compromised, it will not affect the Active Directory domain admin account.

+   - Enable LSA protection. This setting applies to all Windows operating systems and ensures that you cannot attach a debugger to the LSASS process and stops common tools such as Mimikatz from accessing the process. You can read more about how to enable the feature at https://docs.microsoft.com/en-us/windows-server/security/credentials-protection-and-management/configuring-additional-lsa-protection.

+   - Ensure that critical security patches are installed on domain controllers. As I mentioned in the first chapter, some ransomware attacks have been exploiting vulnerabilities such as Zerologon to gain access to the domain controllers. While that vulnerability has now been fixed, there might be new vulnerabilities that appear later with the same severity, so ensure that all security updates are installed on your domain controllers.

+   - Ensure the LDAP server signing requirements are set to require signing. This will ensure that attackers cannot use privilege escalation tools such as KrbRelayUp. This is described in more detail by Microsoft at https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/domain-controller-ldap-server-signing-requirements.

Additionally, if you have the licenses, consider implementing Microsoft Defender for Identity, which provides a cloud-based security service to monitor Active Directory. This service looks at both events and network traffic to detect abnormal user behavior or machine traffic aimed at domain controllers.