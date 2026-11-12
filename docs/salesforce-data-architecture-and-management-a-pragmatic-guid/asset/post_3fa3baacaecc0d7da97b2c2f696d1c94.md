
AMS has grown naturally as well as via acquisitions, resulting in a multitude of CRM, marketing, and ERP systems within its technology landscape. The data architect team has worked to consolidate the various CRMs into Salesforce and Salesforce is now globally used. The company uses multiple web-to-lead forms to capture data from prospects and create lead records in Salesforce. A team of **Sales Development Representatives (SDRs)** also creates leads in the system and routes them to the sales team.

AMS has upward of 3 million leads and 1 million opportunities in its system. For support purposes, AMS has implemented Service Cloud and uses communities to support customers. It also has a Social Customer Service setup that allows customers to open cases with the company via Twitter and Facebook. It also uses web chat, allowing customers to create a case when the regional customer support center is offline. The last count revealed that there are a large number of open cases in the system. A preliminary analysis concluded that some of these were attributable to duplicate cases that were erroneously created.

## Problem

Various teams that use the system have been complaining for months now about incomplete data that is critical for marketing or sales teams finding out that contact records are stale and not reliable except for new customers that have been onboarded. In some extreme cases, sales reps have reached out to a contact only to find out that they have moved and are not with the company anymore. This is causing delayed sales cycles, reduced upsell opportunities, and difficulty retaining customers, leading to higher customer churn rates that are much higher than the industry average.

Duplicate data is rampant in Salesforce, leading to a single customer having their billing and order data spread over multiple accounts. This causes issues with any meaningful reporting related to customer spend, retention, and where the company should focus its energies to increase revenue. Due to this and the data quality issues mentioned earlier, automated processes that run are also aggravating the situation since they are working off of data that is not very reliable. A data architect has been hired to present a plan and implement a governance structure to rectify these issues.

## The solution approach

The new data architect has had initial meetings with different business teams as well as the management team to understand the issues and what could be causing them. The data architect has also had implementation documentation on the CRM consolidation project and the initial setup document when Salesforce was implemented 7 years ago shared with them.

Armed with all this information and after reviewing the documentation, the data architect plans to approach the problem in two steps. Step 1 will be to conduct an assessment of the current state and document and present the assessment report. Step 2 will be to fix existing issues and put governance processes and tools in place to prevent and minimize data quality issues in the future.

## Step 1 - assessing the current state

The data architect wants to ensure that there is a clear scope defined for what's included in the assessment. Based on the information gathered so far, they put together a scope document outlining the boundaries of the assessment, the exit criteria, the assessment team, the approach they will take to conduct the assessment, and any assumptions and constraints that you have identified so far.

Once the scope document has been reviewed and approved by the management team, the data architect conducts a stakeholder analysis exercise identifying the business users, their interests and influence on the project, and the best ways to communicate with these users.

The architect has already reviewed the provided documentation and plans to conduct 1:1 meetings and run some surveys with the identified users. After reviewing the survey results and the notes taken in these meetings, the architect decides to create a business requirements log that will have all of the requirements identified along with the pain points that the users mentioned.

The data architect prepares the assessment report outlining the pain points, the business requirements, and the suggested approach to eliminate the pain points and meet the business requirements. The architect has also suggested some metrics associated with measuring data completeness and accuracy that can be used to measure success in the next phase.

## Step 2 - fixing current issues and implementing a plan

The data architect has recommended a de-duplication tool on an immediate basis to fix the existing duplicate records. The recommended tool also has the functionality to set up duplicate rules that will help prevent new duplicate records from being created. Another tool has been recommended by the architect to enrich the data from data management companies that provide data enrichment services.

The data architect has put together a vision document that outlines the business objectives of the governance plan, the approach, the teams, and the responsible roles to see the plan through success. They have also suggested some metrics to ensure success is continuously measured throughout.

The data architect is also working with the internal communications and training teams to devise a plan on how to communicate the value of data and emphasize that data is everyone's responsibility. In parallel to that, the architect worked with the Salesforce administrators to identify opportunities to streamline page layouts and introduce validation rules so data is correctly entered at the source. Another critical aspect that the architect noted was that there was a lack of proper backup and recovery strategy for Salesforce. The architect has recommended a tool for that and suggested an approach to the Salesforce admins to ensure that backups are taken regularly and restores can be done when needed.

The data architect has engaged the enterprise architect team to put in place a process to ensure that a business architecture for the existing landscape as well as a roadmap is defined. It was found during these discussions that business requirements and enhancement requirements for existing Salesforce applications were not recorded in a central place. To fix this problem, the architect has recommended implementing a business backlog along with a recurring meeting with business users to prioritize the backlog and report on progress.

With these changes in place, the architect has created several Salesforce reports and uses reports by the tools implemented to gauge the quality of data and reports these along with the agreed-upon KPIs to the management team. Phase 1 of the project is complete, and users are already reporting increased satisfaction with the data they are working with along with a decrease in duplicate records, which they were frequently coming across earlier. With the data enrichment tool in place, contact and lead records have more up-to-date and accurate data in them, reducing the time sales reps had to spend earlier to find that information.

## Summary

We have discussed a lot of topics in this chapter. We started off with enterprise data governance and explained what it is and what's different about it with respect to data management. My hope is that our discussion on data governance benefits and the risks of not having it provided you with a deep understanding and appreciation of its importance to any organization regardless of its size. We also highlighted why metadata management, an essential component of data governance, is important and how not having effective governance around metadata management can hinder our ability to govern data efficiently.

Once we understood the importance of data governance, the next logical step was where we go from there and that's what was covered: assessing your current state of data governance. We discussed some challenges with current state assessments, and I provided practical steps that you can take to conduct a successful assessment exercise. We ended the section by discussing some common metrics that can help you evaluate your current state and also set targets for the future state.

In the final section of the chapter, we discussed privacy laws in detail. My hope is that after reading that, you have developed an appreciation of how critical these are to businesses not only for compliance reasons but also for protecting the customer's information and to establish trust in them. Because we live in a global economy, GDPR and now the CCPA are probably the most commonly applied laws. GDPR because it covers the massive economic region of the EU, and the CCPA because it is one of the largest economies in the world. Additionally, we discussed tools and techniques that you can use right away in Salesforce to design and implement compliance with these laws.

In summary, this chapter should have provided you with the knowledge and the tools needed to implement data governance initiatives, what they entail, and how they can be implemented. Starting off with the suggested approach for the current state assessment will enable you to focus on the areas that need the most attention. Data privacy laws are becoming more important and our discussion on those should give you the confidence to have those conversations and implement them at your organization.

In the next chapter, we will discuss performance on the Salesforce Platform and the different tools that are available for monitoring performance, and how we can use them to design solutions to ensure that our applications are scalable and performant.

## Questions

- 1. **Precision Printing (PP)** has been having severe data problems and they have recently hired a data architect to help them clean up their sales master data. The architect ran reports and after analyzing the data, presented a report that pointed out major problems with the data. Thousands of records were identified as duplicate or incomplete. What steps can the data architect recommend to clean up the data and maintain it in a good state? (Choose three answers)

  - a) Hire interns and task them with identifying duplicate records and deleting them manually.
  - b) Use a third-party tool to validate the data.
