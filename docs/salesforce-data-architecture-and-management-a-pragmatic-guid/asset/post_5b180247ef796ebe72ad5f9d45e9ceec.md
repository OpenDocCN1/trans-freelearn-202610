
There are other factors that will influence your strategies, such as data security, scalability (think large data volumes), compliance, and regulatory policies. The backup strategy is also driven by the nature of the business. Consider, for example, the fact that RTOs and RPOs for a global e-commerce site will be very sensitive compared to those for a website for a small single-branch laundry business.

In this section, we will review some of the key considerations for a backup solution. This is intended to be used as a guideline and, depending on the industry of your organization, you may have some other considerations when selecting a backup solution. There are many backup solutions on the market and unless you have very specific requirements that cannot be met by these, chances are you will be implementing an off-the-shelf solution (the buy versus build principle). I have listed the considerations here, bearing this in mind.

## Costs

This is usually the first aspect that comes to mind when looking for a backup solution. Although it’s an important factor, you should really look at the cost from an investment point of view, in other words, what is it going to cost me to make an investment to protect my data? Some backup solutions require a simple setup, but others may require expertise that you may need to pay for, so take that into consideration. Another factor related to cost is understanding the technical and contractual limits of the solution. For example, are there any limits and associated costs for going over a certain storage limit? Do you need to purchase API calls from Salesforce in case you are using a Professional edition or if you reach limits with an API call? Also ascertain whether there are any infrastructure-type investments you need to make, for example, buying additional Amazon or Google storage to host your backups.

## Security

This should be a top concern when choosing a backup solution because you want to ensure compliance with all federal, local, and any policies defined by regulatory bodies. Some other things to consider include whether the backup is encrypted at rest and whether you use your own encryption keys. The location of where the backup is stored is another important consideration. Usually, government agencies in Canada prefer not to have their data stored in the USA due to certain US laws that allow access to that data.

Another important factor is whether the data is stored on-premises or in the cloud. If you are using a third party recommended by Salesforce, does the said third party maintain its own data centers or procure services from another vendor. Physical access to the data is another factor, so you would want to consider who can access your data and what controls are in place to protect this data from unauthorized access.

Can you readily access audit logs, not just for internal controls, but also for when your systems are audited? What is the retention period for backups, and does it align with local laws and policies defined by your organization's privacy officer? What authentication mechanisms are in place to access the backup and do they align with your enterprise's security policies?

## Service levels

You want to ensure that the solution is capable of taking backups in a reasonable amount of time. In large organizations with users spread across time zones, if the backup is taking too long and is not properly optimized, performance can be impacted and slow down operations for business users. What are the RPOs and RTOs defined for the service, and do they align with your disaster recovery plans?

Another consideration is support, whether you are using a third-party backup solution or hosting your backups in a data center. What are the SLAs around that and do they align with your organization's expectations? When using a third-party solution, what kind of monitoring is in place to ensure that backups are not failing due to, for example, infrastructure issues?

## Ease of use

These are considerations related to the use of the solution. You want to understand how much manual intervention is required for the backup because ideally, you want to set it up once and then review your configuration and perform any necessary tweaks once every few months.

Another key consideration is the frequency of backups. Can you set up on-demand, hourly, daily, weekly, monthly, or annual backups, and how much involvement would this require from you? This can be useful when you back up your production environment before a Salesforce release is applied to it. Given that the dates for the next three releases are published in advance, you can easily set up your schedules to take place specifically before the release gets applied to your organization.

You also want to understand the architecture of how the backup solution you are planning to implement operates, because typically they require external fields on the objects you want to back up. Would you need to create these fields or does the solution create the fields for you? Although Salesforce makes allowance for a large number of external fields (25 at the time of writing) per object, you still want to confirm that there aren't any objects in the organization that have reached the limit or will exceed it if your selected solution is implemented.

The key users of your backup solution are Salesforce administrators, and you want to make sure that the solution is intuitive enough and not overly technical so that admins without in-depth technical knowledge can still use it. Another consideration is the availability of material to train new admins and understanding the ramp-up required to train new users.

One important factor is that the solution should be able to detect metadata changes on the Platform automatically or with minimal effort. For example, if new fields are added to an object, the solution should be able to add those fields for backup without requiring extensive intervention. This now brings us to versioning, which is a key factor when restoring data. For example, you made a change to a field from a text to a number and then to a phone type, and each time you took a backup before making the change. You now have a need to revert the field to text, along with the data that was there. This example demonstrates the need to have robust versioning available with the solution that you implement. Ask the solution vendor how they support versioning over multiple API versions, especially over releases.

## Solution comprehensiveness

This is related to understanding how complete the solution is with respect to its offering. We will discuss in more detail the types of backup and restores in later sections, but things to consider here include whether the solution can back up data and metadata:

- **Data and metadata backups**: Some solutions only offer data backups, which then necessitates the need to get another solution or develop one internally for metadata backup. Similarly, can the solution perform both data and metadata restores, and how complete are they? Are big objects supported in the backup, and so on? One solution I reviewed in the past offered limited metadata restore capabilities, which meant that we had to implement processes for the metadata types that didn’t back up for a comprehensive restore strategy.

- **Multi-orgs and Salesforce releases**: You will also want to consider how the solution works with multi-orgs if that is your requirement and how it works with Salesforce releases. Understanding the process your solution vendor takes to ensure its product will work with the next release is crucial as well. I have been in a situation where a vendor for a DevOps tool took more than 4 weeks following a major Salesforce release and during that time, automated processes for **Continuous Integration (CI)** stopped working. The chances of this happening are less with a backup solution due to API backward compatibility, but you should review this thoroughly with your backup solution provider.
