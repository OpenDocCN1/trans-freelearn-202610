
## Step 2: Configure actions
In this step, select **In alarm** under Alarm state trigger.

Click **Create a new topic**.

Provide a descriptive name under **Create a new topic**... For example, you can provide a name such as `TodoPlus-Billing-Alarm`.

Provide an email address where you would like to receive the alert, and then click **Create topic**, as per the following screenshot:

![](img/34ff9b5b5a734dae3450ab701e223c73_63_0.png)

Figure 1.24 – Creating a notification topic and defining an email endpoint

Scroll to the bottom of the screen and click **Next**.

## Step 3: Add name and description
In this step, you will define the alarm name and an optional description.

Click **Next**.

## Step 4: Preview and create
In this step, you will review the settings and then click the **Create alarm** button at the bottom of the screen.

9. Although you have now created the alarm, you will not be able to receive the email alerts until you verify your subscription to the notification topic. To do this, you must log in to the email account that you specified in the previous steps and confirm your subscription.

In the next section, we will provide a summary of this chapter.

# Summary

This chapter discussed the importance of a multi-account architecture, examining the vast use cases for setting up and managing multiple AWS accounts. It falls within *Domain 2: Security* in the *AWS Certified Developer Associate exam* with a particular focus on the task statement, *Implement authentication and/or authorization for applications and AWS services*. Specifically, you learned that hosting different workloads and even different environments in separate AWS accounts is essential for ensuring higher levels of security and reducing the impact of mishaps in one environment affecting workloads in another. With multiple AWS accounts, you can separate individual applications and even have a separate account for their development lifecycle.

With multiple AWS accounts, you can also separate internal applications and workloads from customer-facing ones, further enhancing security. In this chapter, you learned how to manage AWS accounts using AWS Organizations. You learned how to apply service control policies to prevent member accounts from specific services, resources, and regions. The management account in an AWS organization has the highest authority, and you can even prevent administrators of member accounts from leaving the organization.

Although AWS Organizations offers the ability to build guardrails for your member accounts, who or what can work with those permitted services and make API calls still needs to be defined using AWS IAM or by defining resource-based policies. We perform a deep-dive study of the IAM services in the next chapter.

# Further Reading

The following are must-read resources to understand the approaches you can use in the real world when designing a multi-account architecture for your business and clients:

- *Organizing Your AWS Environment Using Multiple Accounts*: `https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html`
- *Service Control Policy Examples*: `https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_examples.html`

# Exam Readiness Drill – Chapter Review Questions

Apart from a solid understanding of key concepts, being able to think quickly under time pressure is a skill that will help you ace your certification exam. That is why working on these skills early on in your learning journey is key.

Chapter review questions are designed to improve your test-taking skills progressively with each chapter you learn and review your understanding of key concepts in the chapter at the same time. You’ll find these at the end of each chapter.

> **How to Access These Materials**
> To learn how to access these resources, head over to the chapter titled *Chapter 17, Accessing the Online Resources.*

To open the Chapter Review Questions for this chapter, perform the following steps:

1. Click the link – `https://packt.link/DVAC02_CH01`.
   Alternatively, you can scan the following **QR code** (*Figure 1.25*):

![](img/34ff9b5b5a734dae3450ab701e223c73_65_0.png)

Figure 1.25 – QR code that opens Chapter Review Questions for logged-in users

2. Once you log in, you’ll see a page similar to the one shown in Figure 1.26:

![](img/34ff9b5b5a734dae3450ab701e223c73_66_0.png)

Figure 1.26 – Chapter Review Questions for Chapter 1

3. Once ready, start the following practice drills, re-attempting the quiz multiple times.

# Exam Readiness Drill

For the first three attempts, don’t worry about the time limit.

## ATTEMPT 1

The first time, aim for at least 40%. Look at the answers you got wrong and read the relevant sections in the chapter again to fix your learning gaps.

## ATTEMPT 2

The second time, aim for at least 60%. Look at the answers you got wrong and read the relevant sections in the chapter again to fix any remaining learning gaps.

## ATTEMPT 3

The third time, aim for at least 75%. Once you score 75% or more, you start working on your timing.

> **Tip**
> You may take more than **three** attempts to reach 75%. That's okay. Just review the relevant sections in the chapter till you get there.

## Working On Timing

**Target**: Your aim is to keep the score the same while trying to answer these questions as quickly as possible. Here's an example of how your next attempts should look like:

| Attempt | Score | Time Taken |
|---|---|---|
| Attempt 5 | 77% | 21 mins 30 seconds |
| Attempt 6 | 78% | 18 mins 34 seconds |
| Attempt 7 | 76% | 14 mins 44 seconds |

Table 1.1 – Sample timing practice drills on the online platform

> **Note**
> The time limits shown in the above table are just examples. Set your own time limits with each attempt based on the time limit of the quiz on the website.

With each new attempt, your score should stay above 75% while your “time taken” to complete should “decrease”. Repeat as many attempts as you want till you feel confident dealing with the time pressure.

# Securing Access with AWS Identity and Access Management

Protecting AWS-hosted applications from unauthorized access is of paramount importance for any business. As an organization, you must ensure that only authorized entities can authenticate against your workloads. Furthermore, those entities must only be permitted to create and manage resources and perform tasks for which they are authorized. This concept is the **principle of least privilege (POLP)** and is fundamental to developing a secure environment. This chapter will teach you why authentication and authorization services are crucial. You will understand the importance of providing various access options to authenticate internal users and, where required, offer anonymous public access depending on the application use case.

In this chapter, you will learn about one of the foundational security services offered by AWS, **identity and access management (IAM)**. You will learn how to create users, groups, and roles and how to grant permissions to access services and configure resources using IAM policies. Another service that you must understand is called **IAM Identity Center** (previously known as **AWS Single Sign-On**). This service is best suited to allowing your technical users access to multiple AWS accounts from a single portal, among other features. In addition, you can also use existing third-party **identity providers (IdPs)** that your workforce may be using to authenticate against the AWS platform – this is known as **identity federation**.

This chapter also examines the **Amazon Cognito** service, a serverless and fully managed identity management system that can facilitate authentication and authorization capabilities for your web or mobile applications. This chapter aims to equip you with the knowledge necessary to leverage AWS security services to construct a reliable, robust, and secure cloud-based architecture. Any business with cloud ambitions would inevitably face similar challenges. Your architectural design patterns must always follow a simple principle – “safe by design.”
