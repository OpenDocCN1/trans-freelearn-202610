
# Incident recovery communication

Let us check out this final function!

## Incident recovery plan execution (RC.RP)

In this control family, we will discuss what to do when recovering from an incident. This includes enacting your recovery plan to bring systems back up to a known running state. This will require that you follow your recovery plan, restore your backups, and establish existing or new baselines for your IT resources.

### RC.RP-01

*The recovery portion of the incident response plan is executed once initiated from the incident response process.*

Now that we have implemented our ICA and PCAs, it is time to start the actual recovery process of our incident response plan. This will entail utilizing our recovery plans to get systems back up and running again with newly established baselines. We may need to also include internal and external stakeholders to do this.

As part of your incident response plans, there should be a way to instruct your IT team to begin work on recreating your environment. There should be step-by-step instructions on how to recreate a system or IT resource and get it back to a known running state. This should include procedures on how to rebuild server or network equipment, where to grab the necessary software, and how to install it.

The network and application flow diagrams we previously made will come in handy to help us depict how the system works and its interdependencies. We will need to recreate our environment to those specifications. In addition to recreating our environment, we will need to also plan for our implementation of new systems and processes as part of our PCA.

Our PCA may require us to break apart old architecture and begin anew or augment our architecture due to new applications being used. Maybe we introduce a new control such as implementing vulnerability management or a new firewall. These new systems will need to be installed and configured and we’ll need to ensure they are working as intended.

If a new process needs to be introduced, this would also be the time to do it. This may include additional firewall rules that need to be configured on the device or revisiting how we are patching our systems against vulnerabilities. A new process may include patching more frequently or setting up SLAs for when to patch certain vulnerabilities based on their criticality. All of this is important to understand when implementing new systems into the environment.

### RC.RP-02

*Recovery actions are selected, scoped, prioritized, and performed.*

As we begin our remediation, we must prioritize where we are placing our recovery efforts. We should review our ICA and PCAs and determine what should be implemented first and what can be placed on hold. This could be due to resource constraints or other issues that may be preventing us from implementing what is needed for recovery efforts.

Do a discovery of what is needed for recovery purposes and begin to build out a project plan for what to implement and when. This project plan should include what is important to get back up as quickly as possible and what can be done later on. The plan should include what can be done immediately and what will take time to get purchased and be put into production.

Work with your executive leadership team (ELT) to understand what is important for them to have implemented as well. The ELT and the business may want you to implement multifactor authentication before a new firewall is implemented, for example. These remediation efforts are all based on the amount of risk the business is willing to accept at that time.

There could also be multiple projects going on at the same time. You could have new firewalls going in while you work on process improvements. Plan out your projects so that they are implemented in accordance with how important they are to the business. This will also require that you have the right amount of resources in place as well.

Resources can come in a number of different types but they typically come down to financial or personnel constraints. Your recovery efforts will be dependent on both of these. You may not have the ability to implement a new control system due to budget constraints and it could get pushed back until more funding is available.

It may also come down to the amount of expertise you have on staff as this may require you to outsource the installation of new systems and services. It could also require that staff members are trained on the new systems being implemented. Both of these will require additional funding for the project. You could even have a trusted third party implement a new solution and train your staff at the same time.

### RC.RP-03

*The integrity of backups and other restoration assets is verified before using them for restoration.*

We have discussed the importance of ensuring that backups are performed on a regular basis. It is now time to test whether we are capable of restoring a backup to an IT resource. In addition to knowing whether we can restore a backup, we must also ensure that the files located in the backups are good and not corrupted. Through this process, we will have to rely on our encryption methods to determine whether we can successfully restore a system to normal.

There are several different methods of performing a backup. One of the more popular methods is the grandfather, father, and son approach. The intention of this approach is to perform backups on a regular basis while minimizing the amount of data we back up, reducing storage space.

The grandfather, father, and son approach to backups goes like this. We perform a full backup on a Sunday and incremental backups during the week. An incremental backup grabs only the changes to a given file; this is to reduce the overall amount of disk space being used. Once Sunday comes again, we perform another full backup and discard the incremental backups that were performed during the week. At the month’s end, we do one more full backup and save that backup for the following month. The increments are called the sons, weekly backups are the fathers, and the monthly backups are the grandfather backups.

Each file that is backed up should be hashed to ensure that when there is a restore, we can verify that the file or image that is being restored matches the hash value. Not only should each file be hashed, but the entire backup itself should also have a hash value. Without a hash, we cannot validate that the backup being restored is an identical copy of what is in the backup.

Most of the time, an adversary will want to gain access to your backups and encrypt them with some type of ransomware with the intention of getting your company to pay the ransom payment in order to regain access to your backed-up files. There are several backup solutions out there that will back up your files into an immutable storage format.

Immutable storage is one where the file is written once and can never be overwritten. There are several benefits to this methodology. First and foremost, once a file is written, it cannot be modified in the storage of the backup. This means that ransomware cannot encrypt the files on the storage disk, maintaining the integrity of the backed-up file.

We should also stress the need to test your backups on a regular basis. By testing your backups, you can minimize the amount of time it takes to locate and restore the backups as it becomes second nature. You should also test whether you can restore an image to a filesystem and ensure that it was not corrupted during the process. These extra steps are to ensure that your backup process runs smoothly.

### RC.RP-04

*Critical mission functions and cybersecurity risk management are considered to establish post-incident operational norms.*
