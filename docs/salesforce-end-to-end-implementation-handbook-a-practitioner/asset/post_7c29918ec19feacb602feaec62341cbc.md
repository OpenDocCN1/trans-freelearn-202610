
- **BA and/or QA specialist**: Triage, assess, and categorize reported bugs and also potentially propose solutions to fix actual bugs
- **Development team**: Fix identified and prioritized bugs

- **Define a process to follow**: Establish and communicate how to report a bug and the process to triage and potentially fix the bugs:
  - Share the bug template with all acceptance testing team members. The bug template should – when correctly filled out – make it easy for the BA or QA specialist to triage, reproduce, and classify a bug. It should contain the following information as a minimum:
    - The username of the bug reporter
    - Test case name or ID
    - At what step of the test case the user encountered the bug
    - What the user expected to happen versus what actually happened
    - A link to the record/page
    - Screenshots of the entire browser window, including the URL bar, which sometimes provide the project team with valuable context about the issue and reduce back-and-forth communication with the tester
- **Determine bug classifications and actions for each**: These typically include the following classifications but may vary depending on the nature of your Salesforce project:
  - **Change request**: Your PO should assess and address these by either communicating to the bug reporter that the request is considered and put in the backlog, or that it is not considered and why.
  - **Not a real bug** This could either be due to user error or faulty data. In either case, there is no development required to fix the issue. Your BA or QA specialist should show the bug reporter how they execute the test case and what data to set up beforehand.
  - **Bug**: The BA and developers can assess the bug, determine the reason for the behavior (root cause), find solutions to fix it, and estimate the effort to do so. The PO can review and determine the prioritization of the bug against the other bugs in the backlog.

## Tip

Many bugs reported during UAT are caused by wrong user setup (profiles permission sets, queues, and so on), because test cases are not followed, or because of **faulty test data.**

To minimize the risk of bugs being reported due to these causes, make sure you provide adequate instructions to your testing team and double-check that test data is correctly set up and ready for the testing team. Test data includes the following:

- Salesforce test user accounts to log in with
- Master data, such as accounts and contacts
- Transactional data, such as opportunities, orders, and cases
- Reference data, such as products, price books, and price book entries

Through the course of UAT, you will likely receive a lot of feedback or bugs from your acceptance testing team. Be open to feedback and thank the team for their effort.

## Tip

Srini Munagavalasa has written an entire chapter dedicated to UAT in a Salesforce context. I suggest you check out *The Salesforce Business Analyst Handbook* (https://www.amazon.com/Salesforce-Business-Analyst-Handbook-techniques/dp/1801813426).

Next, let’s make sure you are ready to get your data into Salesforce!

## Checking your data migration preparedness and signing off to go live

You are now in the final stages of the development phase. You have successfully deployed your Salesforce solution and executed SIT and UAT.

In this section, we are going to highlight the key practical considerations to evaluate whether you are really ready to start migrating data into your Salesforce production environment.

## Evaluating your data migration preparedness

Before you embark on migrating your data to production, make sure you have prepared – and practiced – your data migration plan. It should have been done in the development phase.

### Important note

If you haven’t prepared your data migration plan, you need to pause and check out the *Preparing your data migration plan* section in *Chapter 7*, *Building and Testing Your Initial Release*, and get cracking before proceeding.

Great, you have your data migration plan in place! This means you have determined how you will technically get your data out of any legacy systems, other applications, and sources, and you know how and who will be transforming and loading it into Salesforce.

Let’s look at the list of things you can use to evaluate your preparedness to begin migrating your data into Salesforce:

- You have executed successful data migration dry runs in a staging sandbox.
- You have confirmed that your Salesforce solution is deployed to production – including its configuration data.
- You have confirmed with the development team – beforehand – whether automation and validations are designed to handle migrated data or whether they need to be made inactive during data migration. If the latter, you have confirmed who will do so and when.
- You have aligned with and confirmed the availability of IT and business stakeholders to verify and validate your data migration in production. The same goes for the source system experts to be available in case of issues that need resolution by a new source system extraction run.
- If your implementation partner will participate in migrating your data, make sure you comply with any laws applicable to the countries you operate in according to the nature of the data they will have access to and process.
- You have agreed and aligned the amount of time you have for migrating to production – and validated this to be possible in at least one dry run.
- You have a rollback plan in place. No one wants a data migration to fail, but the truth is some data migrations do fail or take longer than expected and acceptable by your stakeholders. So save yourself a headache and determine what you will do in the event the migration cannot be signed off.
- You have communicated the data migration timing and impact on any system downtime to the teams who are about to go live.

## Tip

Read *Salesforce Data Architecture and Management* by Ahsan Zafar (CTA) to understand and fill in the details of your data migration strategy, plan, and execution.

You have now migrated your data to Salesforce and wish to proceed with training your users and start having them use Salesforce to deliver better experiences for your customers, partners, and employees.

Before you let your users into Salesforce, let’s do a final quality check.

## Signing off to go live

You will be ready to let users go live in Salesforce when you can tick off the following items:

- User acceptance testing was completed with a successful outcome.
- Your Salesforce solution is deployed to production and is verified.
- Your data migrated to Salesforce is validated by both your IT team and the business stakeholders who are going live.
- **User training** is completed for the people about to go live. To minimize disruption to business operations, training sometimes happens in parallel with deployment and data migration. We’ll cover user training in *Chapter 10, Communicating, Training, and Supporting to Drive Adoption*.
- Your users are **provisioned** for your Salesforce production org.

Well done! You have reached the end of deploying your release and migrating your data to Salesforce.

Let’s wrap up this chapter!

## Summary

In this chapter, you have covered a lot of material. We began by guiding you through the key activities and milestones in the roll-out phase. Next, we covered the planning and steps for deploying your Salesforce solution through environments.

You learned how to perform SIT and UAT to make sure your solution works as intended from end to end and is accepted by representatives of your target end-users. Finally, we covered how to evaluate your preparedness for migrating your data to production and provided a checklist for going live.

Having deployed your Salesforce solution to production, you are now ready to train your end-users to set them up for success and to communicate with and support them as they learn to use your solution. All that and more to come in the next chapter of your Salesforce implementation.
