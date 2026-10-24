Chapter 5, Managing Cloud Services with Chef, covers integrating with various cloud providers such as AWS, Rackspace Cloud, and Azure.

Chapter 6, Going Beyond the Basics, focuses on the integration of existing systems in heterogeneous networks, how to deal with recipes and multiple platforms, testing, organization, and publishing of recipes.

# # What you need for this book

This book expects that you have access to some important components in order to be successful. In order to execute the examples in the book, the following prerequisites are needed:

+   - Access to a Chef server for managing your configuration; a self-hosted installation or a Chef-hosted account will work equally well
+   - A workstation where you can install software including knife (Windows or Linux host)
+   - A text editor of your choice

Additionally, if you wish to try out any of the cloud-computing examples, you will need an account with the cloud hosting providers you are trying out.

# # Who this book is for

This book is designed for system administrators who have had some exposure to Chef, either casually or in-depth. It covers the Windows-specific facets of Chef and expects that you have a working installation of the Chef server available for you to use, either hosted or self-managed. A working knowledge of some programming language, preferably Ruby, will be needed to get the most out of the examples and to build your own recipes.

# # Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

Code words in text, cookbook names, recipe names, scripts, database table names, folder names, filenames, file extensions, and pathnames are shown as follows: "In the same way that you would leverage `knife ssh` for Linux systems, `knife winrm` is available to execute commands remotely on a Windows host using the WinRM protocol."