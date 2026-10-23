Correspondingly, there are two parts to the CE configuration for the CSE: configuration for the entire Domain (mainly communication parameters) and configuration per Object Store (Document classes and properties to be indexed).

# Configuring the P8 Domain

Let's begin with the Domain configuration:

+   1. Start up the FileNet Enterprise Manager (FEM) application on your Microsoft Windows machine, and log in as the gcd_admin user. Select the top-level (Domain) node in the tree, right-click, and select Properties.

+   2. From the pop-up panel, select the tab Verity Domain Configuration. Enter the host name and port number for the Verity Master Administration Server that we created in the previous section. We used host wjc-rhel.example.net and port 9950. The credentials are the same k2_os_user credentials that we used to log in to the K2 Dashboard earlier. Click Create Configuration. The additional K2 server names that appear will look familiar from the previous section.

![](bbox=[0.27, 0.43, 0.7, 0.83])