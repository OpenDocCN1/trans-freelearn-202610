# # Backup and security considerations

Backup and security are the two things that are most likely to suffer if you take on the responsibility of an On-Premise deployment. If this is the deployment option you are considering, and you have little experience with a server's backup and security issues, then this is an important section for you.

Clearly, once you get your dream CRM up and running, you will want everyone in the organization to be using it and will want to make it a central data hub for your business. The nature and importance of the data that your CRM system will house makes it imperative that you take the necessary precautions to make sure that the system does not fail, which may in turn produce prolonged down times or worse, data loss.

Accordingly, you will need to devise a data backup strategy to protect the data. One solution is to buy a data backup device — which typically will be tape-based, but could also involve network-attached disk storage. Most backup solutions can be a bit expensive, but, adopting a reliable backup plan is completely mandatory. Losing all your data would be a much more expensive venture.

Some inexpensive backup solutions do exist, such as connecting an external USB hard drive to your server and backing up the server hard drive to the external hard drive. In a similar manner, you can also buy a second drive for your server and backup the main drive to the secondary drive. Note that files that are open at backup time will most likely not be backed up by a simple operating-system level file or drive copy – so be _very_ careful.

Many server administrators will opt to use an internal tape backup unit with proper tape backup software that performs a daily incremental backup and a weekly full backup. Rotate the tape used in the tape drive each week and keep at least 5 tapes in the rotation. Testing of said backups should occur once a month. This can be easily accomplished by identifying a document that has been recently created and checking whether or not it can be successfully restored from the backup.

Regardless of which option you choose, make sure that you keep a copy of your backup at a safe location, away from your office, such as at a bank safety deposit box, safe at your home or elsewhere.