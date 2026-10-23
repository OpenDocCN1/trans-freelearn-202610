VI. Under **Availability Zone**, you can choose an Availability Zone or leave it as **No preference**, as per the following screenshot:

Existing VPC security groups
Choose one or more options
Data-Security-Group ×
Availability Zone Info
No preference

Figure 6.25 – Connectivity option for your RDS database

+   11. Under **Database authentication**, ensure that **Password authentication** is selected.
+   12. Next, expand the **Additional configuration** subsection and:
+       I. Set the Initial database name to `tododb`.
+       II. Leave the **DB parameter** group and **Option** group at their default values.
+       III. Next, under **Backup**, ensure that **Enable automatic backups** is selected and set the **Backup retention period** to 1 day.
+       IV. Leave the **Encryption** and **Maintenance** parameters at their default values.
+   13. Scroll to the bottom of the page and click the **Create database** button.