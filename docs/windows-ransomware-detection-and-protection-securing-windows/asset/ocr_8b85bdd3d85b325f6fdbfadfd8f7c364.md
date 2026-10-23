While there are many different backup products and vendors, there are some general best practices that you should follow to protect your backups from ransomware attacks:

+   - Follow the 3-2-1 rule for backups, meaning that you have three copies of your data across two different mediums, which can be disk and tape or disk and cloud-based backup. Lastly, have one copy of the data offsite.
+   - Ensure that your backup server is not directly accessible from your main environment; keep it in a separate management domain. If possible, make sure that your backup server is not directly accessible from the virtualization layer. There have been cases where attackers have been able to access the backup server through the vSphere Client.
+   - Ensure that your backup data can be stored on ultra-resilient media. We have also seen cases where the backup data was encrypted since the data was stored on regular Windows file servers. Most backup vendors today support the use of immutable backups, meaning that once the backup data has been written, it cannot be modified. For instance, Veeam Backup & Replication has a feature called hardened repositories, where data is stored on a Linux file server using the XFS filesystem, or you can use cloud-based services such as S3 object storage as well.
+   - Having the backup service disconnected from the AD domain also helps reduce the risk of the backup server being impacted if attackers manage to compromise your AD.
+   - Ensure that you enable encryption on the backup jobs. This ensures that if someone manages to gain access to the backup data, they won’t be able to read the encrypted data.
+   - Finally, it is crucial to regularly check the status of your backups and confirm that they are being executed successfully. Additionally, it is essential to test the integrity of your backup files to ensure that the data can be restored correctly in the event of a disaster. To do this, you can use various tools, such as checksum or file comparison software, to verify that the backup files match the original data. It is also recommended to perform regular test restores to ensure that the backup files can be successfully restored in the event of an emergency.

In regards to checking the status of backups, there are many vendors that have built-in backup verification features. Many of these backup products can automatically spin up virtual machines in an isolated network based on the backup data and run tests to verify the integrity of the data.

# Summary

In this chapter, we learned how to configure and use AIP to classify and encrypt data to minimize the risk of malicious actors getting access to information.

We also investigated using other features to encrypt other data sources, such as SQL Server and file servers.