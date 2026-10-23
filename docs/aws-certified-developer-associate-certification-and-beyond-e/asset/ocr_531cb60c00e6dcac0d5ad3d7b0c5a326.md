28. Under the VPC security group (firewall) section, ensure that Choose existing is selected, and from the Existing VPC security groups drop-down list, select the security group you created in Project Task 10.2 for the database, which should be Data-Security-Group. Also, ensure that you deselect the default security group from the list as per the screenshot:

![](bbox=[0.15, 0.2, 0.86, 0.44])

Figure 10.17: VPC security group (firewall) configuration for your RDS database

29. Select the eu-west-2a AZ from the Availability Zone drop-down list.

30. Scroll down further and ensure that Password authentication is selected in the Database authentication section.

31. Scroll down and expand the Additional configuration section. Set Initial database name to tododb.

32. Under Backup, select the checkbox next to Enable automatic backups and set Backup retention period to 1 day.

33. Leave all the remaining settings at their default values and scroll to the bottom of the page. Click the Create database button. Your MySQL database will now be provisioned. This can take a few minutes to complete. Once complete, you should see that the database is now available, as per the following screenshot: