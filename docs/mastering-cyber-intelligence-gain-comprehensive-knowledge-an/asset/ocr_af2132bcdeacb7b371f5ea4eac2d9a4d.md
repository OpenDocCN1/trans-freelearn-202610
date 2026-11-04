# Auto system behavior for user access limitation

First, imagine a user with access to sensitive data they are not supposed to have (high-level privilege for low-level users). Second, imagine a user has access to one database and automatically gains access to another database they are not supposed to access. An example would be a software developer who can access the employees' database, contracts, and salary information. Why would a software developer access contract and salary data? Because they can – they have been given access. Excellent usable security must do the following:

+   - Allow the user to *reduce their access privilege without much effort*. This option is dangerous if access has been compromised. No adversary will want to minimize access privilege to conduct malicious activities. Adversaries always aim for local administrators or the highest privilege.
+   - Automatically restrain the user from accessing the information with the correct message (for instance, *Access denied. No permission to access the data*). This is the recommended option.

With technology and design advances, modern systems are built with effective permission and data access control. And both users and systems need to be monitored to ensure that best security practices are followed.

User interaction and system behavior always work together to provide a better experience. Because users do not always use the best practices, it is essential to consider systems and solutions that provide sound security while accommodating users.

This section has discussed four essential usable security guidelines for system design and development. And from a CTI analyst standpoint, modeling threats applicable to system designs and integrating the recommendations in the development process is an excellent practice to help organizations with secured but usable systems shift from DevOps to DevSecOps. The following section looks at usability in the authentication process, an integral part of all systems and applications.