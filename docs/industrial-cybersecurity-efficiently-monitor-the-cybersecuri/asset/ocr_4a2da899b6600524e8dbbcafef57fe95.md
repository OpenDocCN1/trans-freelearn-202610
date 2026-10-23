Follow these steps to find all the domain users and perform a password spray on them:

+   1. We will use Windapsearch to ask the domain controller for all the users in the domain:

![](bbox=[0.13, 0.18, 0.88, 0.4])

Figure 17.24 – Windapsearch domain users

Aim the command we used at the OT-domain DC (-d 172.25.100.100), log in with the credentials for user-1@ot-domain, and ask for the domain users with -m users. Then, pipe the output of windapsearch into a grep command, to filter out the sAMAccountName lines (the account names), stripping off all the fluff output. Next, we must pipe the output from grep into AWK so that we only display the username string (print $2). Finally, we must send the output to a file with > users.txt. At this point, have a list of domain users to feed to the Kerbrute password spray tool.

+   2. Kerbrute is a tool that can be used to quickly brute force and enumerate valid Active Directory accounts through Kerberos's pre-authentication functionality. The tool can be installed by running the following command (with the Go language installed):

```
go get github.com/ropnop/kerbrute
```