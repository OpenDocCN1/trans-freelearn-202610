Administrator

QT-DOMAIN\Domain Admins

The command completed successfully.

Additionally, we are interested in the **power users** group:

C:\Documents and Settings\Administrator>net localgroup "power users"

Alias name  power users

Comment  Power Users possess most administrative powers with some restrictions. Thus, Power Users can run legacy applications in addition to certified applications

Members

----------------------------------------------------------------------------- -------

The command completed successfully.

Finally, if we want to see whether any groups were created that shouldn’t have been, we can use the net localgroup command without specifying a group:

C:\Documents and Settings\Administrator>net localgroup

Aliases for \\HMI-1

----------------------------------------------------------------------------

*Administrators

*Attackers

*Backup Operators

*Guests

*HelpServicesGroup

*Network Configuration Operators

*Power Users

*Remote Desktop Users

*Replicator

*Users

The command completed successfully.