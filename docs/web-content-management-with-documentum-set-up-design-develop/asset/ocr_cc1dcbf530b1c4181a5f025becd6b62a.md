# 25.4 Summary

Documentum applications, like any other software applications, need to be developed on a development environment, deployed on a separate test environment for system testing purposes, and eventually deployed on to the production environment.

In Documentum, the following entities need to be migrated (deployed) from one environment to the other—Docbase objects in a DocApp, existing content, Web Publisher or any other client application code/customized files, and any deployment scripts for the target environment.

In this chapter we briefly discussed the following existing migration methodologies in Documentum, their benefits and drawbacks—DocApp migration, Dump and Load scripts, Documentum FTP Services, custom-written scripts, manual deployment, and automated mechanisms through ANT scripts.

Using Documentum Application Builder and Documentum Application Installer tools, we discussed in detail how to migrate DocApps from one environment to another by setting correct installation options.

Also, we briefly discussed an example using Apache ANT to automate migration of Web Publisher/DFC/customized code files to a target server.