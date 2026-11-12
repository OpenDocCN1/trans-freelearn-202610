Managed Service Accounts (MSAs) are AD DS object classes that provide this service account capability; they benefit from lowering admin effort for Service Principal Name (SPN) management and service account password management.

There are two types of MSAs, as follows:

+   Standard Managed Service Accounts (sMSAs) are local to a computer, service, or application and cannot be shared across other resources in the network. This limits their efficiency and adds an administrative burden. Needing to implement multiple local sMSAs increases the threat, security, and configuration drift surface area and exposure.

Further information about sMSAs can be found at https : // docs . microsoft . com / en-us / azure / active-directory / fundamentals / service-accounts-standalone-managed.

+   Group Managed Service Accounts (gMSAs) have domain scopes and capabilities and can be used across multiple computers, services, and applications; NLB clusters and IIS are examples.

gMSAs require you to create a key distribution services (KDS) root key on a DC to create gMSA accounts.

The following are some AD PowerShell cmdlets that can be used to manage gMSAs:

+   - Get-ADServiceAccount
+   - Install-ADServiceAccount
+   - New-ADServiceAccount
+   - Remove-ADServiceAccount
+   - Set-ADServiceAccount
+   - Test-ADServiceAccount
+   - Uninstall-ADServiceAccount

Further information about gMSAs can be found at https : // docs . microsoft . com / en-us / azure / active-directory / fundamentals / service-accounts-group-managed.

# Computer objects

Just as users have an account in the directory service so that we can search for and locate these users, we need the same mechanism to search for and locate computers in the domain. A computer object provides an account for a computer; it represents a computer resource on the network and allows it to uniquely identify itself and authenticate. Once a computer has a computer account, it can be managed as a domain resource and configured using Group Policy.

User account objects and computer account objects are the two foundation building block object classes in AD DS.