
## Human errors

Most data loss errors happen because of **human errors**. This can result in the permanent deletion of your data without the person doing it even realizing what they have done. A few years ago, a Reddit post mentioned a user trying to update a few thousand records but they forgot to set the desired record type on those records. The platform automatically sets all the uploaded records to the default record type. When the person realized that the uploaded records had the incorrect record type, they proceeded to run a report for the default record type and subsequently deleted all of the records under the default record type.

The problem was that there were a large number of records with the default record type that also got deleted. The user then uploaded the initial load again with the correct record type. The mistake wasn't realized until the next day when it was discovered that a large number of records were missing. These and other scenarios like this can happen and can be very costly to the business.

## Retired Salesforce data recovery service

As mentioned in an earlier section, if you were counting on having your data recovered by Salesforce, the data recovery service has not been available since July 2020. In this case, if you don't have a backup of your data and lose data, you will have to either rely on old backups (provided you have some) or rekey the data from other sources, for example, invoices and orders, but that may not be possible for leads or for data that is not in other systems. Even if the data recovery service was available, it was a less than optimal solution because Salesforce couldn't guarantee data recovery; it was just limited to transactional records, metadata was not included, and it would take 6–8 weeks to recover that data. The data was also in CSV format, not to mention that it would cost your organization $10,000 per incident.

## The Recycle Bin

Salesforce does have the Recycle Bin, with which you can restore records in the event of adversity, but the Recycle Bin shouldn't be considered a backup solution because it can restore deleted records. If you have unintentionally updated records and want to restore those records, the Recycle Bin will not help you.

It's also worth pointing out that the Recycle Bin holds records for 15 days, after which point, they are deleted automatically. With the Salesforce data recovery service retired, once this happens, there is practically no way to restore that data.

## Salesforce outages

Salesforce outages can lead to data loss, and this has happened in the past. In 2019, an unruly script gave view and modify all data permissions to instances that had integrated with Pardot. This incident came to be known as Permageddon, and caused admins to lose permissions in their orgs. The reason was Salesforce deleted all permissions that were affected to protect customers' data. It was a rude awakening for companies that didn't have metadata backups in place and admins had to manually recreate profiles and permission sets for their organization.

In another instance in 2016, a crash on one of the instances, NA14, caused a few hours of customers' data to be lost and it could not be restored. Salesforce has an excellent track record of protecting its customers' data but, despite the best of intentions, things can happen, and customers should be prepared for all scenarios.

## Salesforce recommends backing up

In this help article (https://help.salesforce.com/articleView?id=000334121&type=1&mode=1), Salesforce recommends that customers develop a strategy as part of data management and back up their data regularly, and the platform has tools that can be used for this purpose. These tools are designed to execute data imports, updates, deletions, and so on, but there are lots of third-party tools that specialize in data backup and recovery and make the process simple and easy to implement.

## Quick recovery from catastrophic events

When a catastrophic event happens, leading to potential data loss, your priority is to restore your business operations to provide an ongoing service to your customers. Having a data backup strategy that is well thought out and tested gives you the peace of mind that recovery will be quick.

## Migrating data

When migrating data, having backups gives you the ability to restore data if the migration goes south. In certain situations, it can also provide you with the ability to retry your loads in a sandbox environment before executing the data migration in a production environment. For example, if you have large amounts of data for a new application that you want to load test before promoting to production, you would need to use the full sandbox, which can only be refreshed every 30 days. If you need to run your migration a few times after tweaking and refining it each time, you can use your backups to restore data in the full sandbox and conduct your testing.

## Compliance and regulatory policies

Depending on the nature of the business, the jurisdiction, and its laws, there may be requirements to retain data for a certain period of time. You could take a backup of data and store it in an offsite or cloud location and reference it when needed. A company had more than a decade of invoicing data in its system and required 7 years' worth of data for compliance purposes. They still wanted to keep the rest of the data, but not on the platform, and used backups in CSV format to back up data that was older than 7 years and stored it in a cloud location. They then performed a few iterations of testing to ensure that their processes and the tools they were using if they needed to reference that data were meeting expectations and then proceeded to remove the data from the platform.

## Threats becoming more sophisticated and malicious

As technology becomes more prevalent and AI becomes more common, data-related attacks will continue to evolve and become more sophisticated and malicious. In 2019, the Global Risk Report listed data theft and fraud in the top five global risks. Having a robust and well-tested backup strategy can help with these threats and ensure the continued operation of your business. A Small to Medium-sized Business (SMB) in 2014 got hit with a ransomware attack, where its previous estimates for the last 10 or so years that were on a senior estimator's computer were lost. There was no backup of the data and therefore no way to recover it, except to pay the ransom, which the company decided not to pay. Given that the company operated in an industry where there were a handful of very large multi-billion dollar customers, and it was critical to have estimates available from previously completed similar jobs, this was a significant hit to the company.

## Reputational risk

There is a real cost associated with reputational risk, just as there is a monetary value linked with goodwill. It can be a bit challenging to put a direct cost on this, but studies show that customers start to move away from companies that they cannot trust. Imagine if, during Permageddon, Salesforce decided not to delete the affected permissions to protect customer data and users had access to unauthorized data, would that be a worse outcome or the fact that the admins had to rebuild permissions for their organization?

## Data loss via integration

Consider a scenario where a company has been preparing for months to launch a product globally with multi-country advertising campaigns and then, just days before the launch date, an integration runs and corrupts some of the data that was to be used for the campaign. Not only would it lead to productivity loss to recover that data from backups (assuming they are there), but it would also delay the launch of the product, thereby impacting timelines and costing the company money and lost revenue.

## Natural disasters

Although this is less likely to happen and something that Salesforce has thought of and is responsible for, natural disasters can, and do, happen. When a natural disaster strikes, you want to have your own peace of mind that your data is backed up and available for restoration.

## Point-in-time recovery

One additional benefit of doing backups is that you can do data comparisons to understand what has changed. For example, field history tracking keeps data for up to 24 months, but if you need to compare data prior to that, that won't be available unless you are using Field Audit Trail, which retains historical data for up to 10 years.

Now that we have looked at the reasons why you should back up your data, we will dive into how you go about doing that.

## Devising a strategy for data backup

A key item to determine as part of a backup strategy is to have a rough idea of how much it's going to cost if access to some or all of the data was lost, and the cost of replacing that data if it is lost permanently. This is where the **Recovery Point Objective** (RPO) and **Recovery Time Objective** (RTO) are important. The RPO defines the amount of data you can lose before a successful restore happens, whereas the RTO defines the amount of time a business can be without access to the data. The first one is a key driver in determining how frequently the backup should be running and the RTO is a key consideration for how much downtime you can afford when you don't have access to your data. It is important to note that the RTO and RPO mentioned here refer to the portion of the data and metadata that the customer is responsible for. Disasters that happen in the infrastructure for which Salesforce is responsible have very efficient RPOs and RTOs because their infrastructure is set up in a redundant configuration.
