
CiviCRM ships with seven pre-configured rules. Let's briefly review their intended use:

- **New members**: These are members who have just joined the organization. Perhaps you want to recognize them by sending them special communications or recognize their "newness" with a flag on their conference nametag. Whatever your use, this status will help you quickly identify new members.
- **Current members**: These are members for whom the current date falls within their membership period start and end dates.
- **Grace**: This status provides a way to give members some flexibility if they have not renewed their memberships prior to their end date. Let's be honest — most members wait until after their end date to renew their membership. You don't necessarily want to cut them off right away; so you configure a grace period in which their member benefits continue, but they are not considered a **Current** member. This can also be helpful for your membership director or membership committee. You can use the **Grace** status to quickly generate a list of members who have not yet renewed for the current year, and start reaching out to encourage renewal.
- **Expired**: Once a member is past the end date and past the grace period allowance, their membership expires. The **Expired** status tells you that they let their membership lapse without renewing.
- **Pending**: The **Pending** status is a special use case when allowing membership signups with the "pay later" option. The "pay later" option can be enabled for member signup pages as an alternative to collecting real-time credit card payments. In effect, it is a promise to pay by check, cash, or whatever other options you provide to members. When a membership record is generated through pay later, both the membership and contribution record is flagged as **Pending**. Once the payment is received, you will record it in the contribution record and mark it as **Complete**. After saving the completed contribution record, its corresponding pending membership will be updated to the **New** or **Current** status.
- **Cancelled**: The first four status types are all date-based rules. Depending on the current date and configured rules, the membership record will be assigned the first matching status. The fifth one (**Pending**) is a payment-based rule. In contrast, the **Cancelled** status is generally only handled on a manual basis. For example, a person may join your advocacy organization and at a later point find that they disagree with certain positions taken by the organization, prompting them to contact you and cancel their membership. There is no date or payment-based calculation triggering the cancelation; you simply change their status to **Cancelled** in the membership record.

## Growing Your Membership and Interacting with Members

- **Deceased:** The last default status type, **Deceased**, may be manually applied to the record or will be triggered by the contact's "deceased" field.

Most organizations will find the basic functionality afforded by these default rules to be sufficient for their needs (though they will certainly want to review them and may find a need to tweak the rules configuration, such as adjusting the duration of their grace period). If the default rules do not meet your needs, you may add, edit, remove, or reorder the rules as needed. For example, you may construct a multi-tiered grace period structure before members expire—grace (one month), limited (two months), last chance (three months), in which you gradually restrict the user from certain member benefits and communicate with them in different ways.

While creating a new rule or editing an existing one, you have several configuration options available. These options can be easily misunderstood or misconfigured, so let's take a moment to review them:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_338_0.png)

Each status rule will define a trigger (**Start Event**) that indicates when the rule is activated, and an optional ending point (**End Event**). Each of these may be offset by an adjustment value. Both events are measured from one of three dates—join date (the initial date the individual joined the organization), start date (the date when the membership period began), and the end date (the date the membership period will end or has ended).

It can be a little confusing to think through how the start/end events, associated with the join/start/end date, translate to your desired status. One way to think through this is to build a simple timeline representing each status:

![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_339_0.png)

Let's take a closer look at how our timeline grid in the preceding diagram impacts the grace status settings. As you recall, the grace status is intended to represent the period after the membership end date, but before we regard the record as expired.

- **Start Event:** End Date, no adjustment
- **End Event:** End Date, 2 months

What does this mean? Starting with the membership's end date, the record will be flagged as grace status for two months. After that period, it will be changed to the expired status, or whatever other rule then applies.

> 💡 Be sure to designate one status as the default one. It will be assigned to a membership record in the absence of another matching rule.

In addition to setting the start and end events, you may consider marking a status as "administrator only" in the **Administrator Only?** flag. Flagging it thus means the status will never be assigned through rule matching. It can only be assigned if an administrator overrides the calculated membership status and assigns it. You may, for example, create an **On hold** status which administrators use to flag members who must submit credentials or other paperwork before being considered **Current** members.

The **Current Membership?** flag indicates which status types should be counted as current members in the dashboard grid displaying membership totals.

After setting up your rules, you may want to run some test records, changing the join, start, and end dates to match the various statuses you expect will be assigned.

> Be sure to set up the cron job, as outlined in the *Configuring cron jobs* section, to trigger automated membership status updates. When run nightly, or on a periodic basis, it will review all membership records against the status rules and change them as necessary. If you do not set up the cron, your membership statuses will only be updated when a membership record is created or manually updated.

## Setting up renewal reminders

By now, you should have your membership types set up, and be planning to create membership join and renewal forms to encourage new and existing members to work directly with the system through your website. Also, you will need to generate your periodic member renewal invoices to retain existing members.

Wouldn't it be great if CiviCRM could know when a membership record is nearing its end date and send the member a reminder e-mail, encouraging them to renew? Well, the good news is that CiviCRM provides just that type of capability! You may have noticed a section entitled **Renewal Reminders** at the bottom of the membership type configuration page. We use that to set up reminder e-mail options. Note that this feature is membership type-specific — you can choose not to enable renewal reminders for certain membership types or to configure different e-mail templates for use with different types.

There are two very important steps that must take place in order to use this feature — you must create your message template(s) and you must set up the cron job script (detailed in the *Configuring cron jobs* section). Assuming you are configuring these options for the first time, you may see a yellow message box informing you that no message templates currently exist and directing you to the appropriate page to set those up. You can access them through the menu by visiting **Administer | Configure | Message Templates.**

The message template, as the name suggests, will have the body text for the reminder e-mail to members and may also contain tokens — placeholders for contact record data. The **Message Templates** page has two tabs — **User-driven Messages** and **System Workflow Messages**. The latter is used for e-mails sent by the system in conjunction with core functions, such as receipt e-mails for contributions. Our concern at this time is **User-driven Messages.**
