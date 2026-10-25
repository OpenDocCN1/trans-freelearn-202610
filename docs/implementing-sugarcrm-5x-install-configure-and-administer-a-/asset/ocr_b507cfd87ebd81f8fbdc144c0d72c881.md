# # Server security

Your CRM server is likely to have a permanent link to the Internet to provide remote users with access to its data while away from their office. The data they will be accessing is perhaps the most vital and sensitive information your business owns. Bells should be going off in your head, warning you to make sure that the server is properly secured, as this combination is too tempting for someone with malicious intentions. Cyber attacks, or the attempted theft of your competitive information are two among the many very real threats for which you must plan.

For a start, security specialists will always tell you that nothing is completely secure. There are simply levels of security —each more cumbersome and expensive than the last— and you need to implement a level of security that is sensible and appropriate in your business context.

Some minimal security measures you should consider include the following:

+   • A UPS (Uninterruptible Power Supply) to save your server from crashing when there is a power outage. Windows and Linux both have utilities that can receive a message from the UPS notifying the server when the UPS has gone over to battery backup, so that the server can be shut down in a controlled manner if the power remains out for too long.

+   • A locked server room, so only authorized employees can access the server, reducing its chances of being stolen, being damaged, or having its data compromised.

+   • A surveillance system is an effective deterrent of theft or vandalism. You should consider installing one that has the ability to monitor your server room.

+   • A VPN (Virtual Private Network) to limit the server’s exposure to the Internet, while simultaneously allowing remote users uninterrupted access to the system.

+   • A firewall between the server and the Internet connection, with only limited and specific access to the web server being permitted from the Internet.

If you are installing the SugarCRM server at your own office, you will typically position the server behind your firewall to protect it from the outside world, but will allow external access by opening ports on the firewall to the web server on your SugarCRM server. From a security point of view, it would be even better to actually have separate web and application servers, with security rules between them — but this is not a necessary security measure for smaller firms.