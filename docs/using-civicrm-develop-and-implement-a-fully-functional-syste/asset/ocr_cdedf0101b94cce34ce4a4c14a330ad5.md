# Configuring CiviPledge

CiviPledge is a CiviCRM component that supports non-enforceable commitments for recurring payments. This is different from recurring contributions that are also supported by CiviCRM, but which actually create an authorization record with the payment processor to automatically transfer funds on a recurring basis. In contrast, pledges are promises to pay a fixed amount through multiple periodic payments.

For example, your organization might encourage existing donors to increase their total annual contribution by committing to a monthly payment which you associate with a corresponding monthly need. A homeless shelter might create a campaign to "provide five meals a month" for the year. The total annual donation is $240, consisting of $20 monthly payments.

When a pledge record is created, it consists of the total committed donations and the terms of payment, that is, number of payments and frequency (12 payments of $20). It is, however, only a promise to pay. CiviCRM provides useful tools for tracking payments, sending out reminders, and providing online payment forms. As the constituent fulfills their commitment through periodic contributions, you log the payment against the pledge record and track their progress. Once all payments are received, the pledge record can be closed.

Before implementing pledge signups, you must enable the component (it is disabled by default). Click on Administer | Configure | Global Settings | Enable CiviCRM Components and move CiviPledge from the left column to the right.

You may configure automated pledge reminder e-mails that will be sent to constituents prior to the due date for their next payment. Enabling this feature is done through the Contribution Page setup, where you also indicate the form that will be used for pledges. However, you must also configure a cron job to periodically check the status and due date of individual pledge records, and trigger the e-mail. See Chapter 3, Installation, Configuration, and Maintenance for a full discussion of configuring cron jobs.

# Recording a contribution manually

Now that we have configured CiviContribute and have a working knowledge of the key functionality options, we are in a position to look at recording a contribution record. Part of the reason for learning how to record a contribution manually at this point is that it will provide a better sense of the wealth of information that is stored about a donation. This will lay the groundwork for a better understanding of the automation that occurs when online contribution pages automatically record contributions.