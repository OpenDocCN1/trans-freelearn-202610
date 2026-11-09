
Accountability Act (**HIPAA**), and so on, this is where you’ll provide this information. If none of these aspects applies to your app, simply check **N/A** and proceed.

- 3. **Questionnaire:** Here, you’ll detail your packaged solution’s architecture and any third-party services it integrates with. Essentially, you’re describing the general architecture environment required by your packaged application.
- 4. **Docs:** This is where you’ll upload code scans, false positives, solution architecture, and use-case documentation you’ve prepared.
- 5. **Test Environments:** Here, you’ll submit credentials for your demo environment and any integrated off-platform service, client, mobile, or desktop app. You’re required to submit a fully configured representative demo environment of your packaged solution.

## Security review operations verification

The security review operations verification phase is a crucial step in Salesforce’s security review process. This phase entails checking for the presence and correctness of related packages in the demo organization, ensuring the submitted version is accurate, verifying the completeness of use-case documentation, and assessing the validity of scans and false positive documentation. Any discrepancies in these areas can cause delays in the review process. Once the verification is complete, you’ll receive an email either confirming your application’s placement in the security review queue or detailing any issues that need resolution:

## Formula Debugger has been submitted for security review

Osobiste/AppExchange Success Blueprint x

noreply@salesforce.com <no.reply@salesforce.com>

wt., 25 maj 2021, 17:33

do mnie

![](img/44ed632038957e4c80220676e84c5109_308_0.png)

Dear Partner,

You're all set! Formula Debugger has been successfully submitted to our Product Security team for review — thank you for your commitment to trust!

## Here's what you need to know about next steps:

- While we strive to deliver results within a few weeks, the actual duration may vary due to a number of internal and external factors (i.e number of other apps in the queue, bandwidth of testers, complexity of your app, etc.).

Figure 6.5: A screenshot depicting an email confirmation indicating that your application has successfully entered the security review queue

## Submitting an application for a security review with a trial PBO

For Salesforce partners who intend to submit a **second-generation package** **(2GP)** for security review and are still operating on a trial PBO, there are certain guidelines and practices to follow. This is to ensure a smooth security review process and prevent potential delays or operational issues.

In the context of the **Partner Application Development Agreement** **(PADA)** and **Salesforce Partner Program Agreement (SPPA)** described in *Chapter 2*, it’s important to note that the completion of the PADA is a key requirement for activating a PBO from trial mode. However, the new security review procedure allows for the submission of an app without having to complete the PADA first.

To avoid problems that may arise from using a trial Dev Hub org for the security review, the security review team has implemented a default policy of blocking 2GPs built on trial Dev Hub orgs from entering the security review. This policy is intended to prevent complications that can occur if a 2GP becomes orphaned due to the expiration and purging of a trial Dev Hub org.

For partners facing this block, there is an option to book a remediation meeting with Salesforce experts to review their submissions. In these sessions, the risks associated with using a non-active Dev Hub org are explained, and clear guidance is provided to rectify any issues identified. If it is confirmed during the remediation meeting that the partner is planning to activate their PBO after executing the PADA, an exception to the “no trial Dev Hub org” policy may be granted. This allows the partner to progress through the security review process while concurrently finalizing the contract with the Salesforce ISV Sales team.

To optimize the security review process, partners without a PADA in place may proactively schedule a remediation meeting before submitting for a security review.

## Failing the security review

![](img/44ed632038957e4c80220676e84c5109_311_0.png)

Alright, dear reader, take a deep breath and let’s have a heart-to-heart chat. Imagine you’re walking the red carpet of AppExchange stardom, and then...you trip. In more professional terms, your application has failed the security review. But before you start seeing that as a tabloid scandal, I want you to repeat after me: “It’s not the end of the world.”

Sure—resubmitting your application means you have to cough up the fee ($999, to be precise) and you’ll have to wait a few more weeks but think of it as your entrance being delayed for a grander reveal. The point is, that this hiccup in your journey to AppExchange fame might affect your **go-to-market (GTM)** timeline, but it’s far from a showstopper.

Now, about the “failure”. That word sounds quite dramatic, doesn’t it? Conjures up images of doom and gloom. But hold on—let’s toss that mindset out the window. Let’s not think of it as failing, but rather as...learning. Salesforce, the benevolent godparent of AppExchange, truly wants you to flourish. However, it also has to keep the interests of its customers in mind, who are the potential audience for your showstopping app.

So, what if it takes a few iterations of your package before you finally pass? Don’t think of Salesforce as a stern judge on a talent show, giving a verdict on your performance. It’s more like a friendly mentor who gives you constructive criticism, wanting you to improve and shine. If you struggle, they’re ready to sit down with you during office hours to give you the helping hand you need.

The security review process? Well, let’s call it what it really is: a security-hardening sprint. The feedback you get from Salesforce is basically your personal trainer, helping you sculpt your app into the perfect shape. So, dust yourself off and get ready for another round—every hurdle is a step toward a more secure app.

Why doesn’t the security review spot all vulnerabilities in the first round? Well, each review is like an episode of a TV show—time-boxed and structured. The advantage? It keeps costs manageable for you and reduces review queue times. But as with any good series, there can be a few cliffhangers. Not every issue is spotted or fully resolved in a single episode.

Why might your app pass the initial scan and fail the deeper review? Think of the initial scan as a general health checkup, while the deeper review is more like a full medical examination. The latter requires more detailed assessment and human expertise, catching more subtle issues.

What if you’ve fixed all the findings but still fail the security review? Imagine you’ve baked a cake and the recipe asks you to remove the lumps in the batter. You find and remove some, but not all. The security review report points out the lumps (security issues), but it may not highlight every single one. After all, creating a smooth batter (secure code) is part of your recipe (development process).

Remember—this journey isn’t about ticking boxes. Security isn’t a task you check off your to-do list. It’s a mindset, a philosophy—a lifestyle, if you will. So, prepare to embrace it wholeheartedly, and before you know it, you’ll be making your grand entrance.

The email shown in the following screenshot contains a report that includes descriptions of vulnerabilities that were found in a failed security review:

## Formula Debugger: Security Review - Your App Did Not Pass

![](img/44ed632038957e4c80220676e84c5109_314_0.png)

Dear Partner,

We have completed the security review of your application. Unfortunately, we have found some issues which concern us, and thus, at this time we cannot approve your application for final listing.

Trust and security are core values at Salesforce, and we are committed to working with you to resolve those issues. The following vulnerabilities need to be resolved:

- Lightning: CSP-Incompatible Javascript

In order to list Formula Debugger publicly on the AppExchange, the vulnerabilities from the report must be fixed.
