# 22

# Web Viewing Content Files

Now that we have created a Site Publishing Configuration and studied how publishing works in Documentum, we are all set to take our discussion a step further and learn about Web View.

Web View is one of the areas least explored by most Web Publisher users and if utilized well, is really useful! Web Publisher's Web View feature lets you preview content files on your web servers so that you can QA it and be assured about its look and feel and other aspects such as links to other pages on your website before publishing to the production site.

Just treat Web View as viewing web content on your web server. Web View is available by default in Web Publisher and figure 22.1 shows how web viewing works in Web Publisher:

![](bbox=[0.17, 0.47, 0.82, 0.58])

Figure 22.1: Web Viewing web-ready content

The web viewing mechanism is fairly simple and the steps are as follows:

+   1. Ensure that SCS Target software is installed and configured correctly on the web server.
+   2. Set up your web server appropriately and point the publish location in Site Publishing Configuration to the home directory of your web server.
+   3. Decide which lifecycle state of content should be web viewed. If it is WIP, then prepare a Site Publishing Configuration for publishing WIP content using Documentum Administrator. (We will cover the details of this Site Publishing Configuration later in this chapter.)
+   4. Prepare the presentation files (XSLs) for the content XML files in Web Publisher so that the necessary renditions can be generated for the content files.