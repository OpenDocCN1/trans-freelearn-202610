
## 8
Use Case 1 – Receipt Processing with Document Understanding

Every organization, irrespective of the industry, has a team that requires receipt processing. Examples of receipt processing can be seen in procurement teams to pay for services delivered, or in HR teams to review expenses submitted by colleagues during business travel. While the use for receipt processing may differ between these teams, one similarity is that processing the receipts is usually a manual and potentially error-prone task to perform. Fortunately, with UiPath's Document Understanding, we can create cognitive automation to apply Optical Character Recognition (OCR) to read, classify, and interpret receipts to eliminate manual work.

By having a full understanding of Document Understanding and the *Document Understanding framework*, we can work together to build cognitive automation that can interpret the images of receipts. As we build the use case, we will follow a similar development life cycle to that outlined in the previous chapters. We will start with understanding the current state of our automation opportunity and then design the future state of the process with automation involved. Once the future state is designed, we will build the automation with Document Understanding, test the solution to ensure stability, and finally, deploy the automation into production.

In this chapter, we will cover the following topics as we build automation to process receipts:

- Understanding the current state
- Creating the future state
- Building the solution with the Document Understanding framework
- Testing to ensure stability and improve accuracy
- Deploying with the end user experience in mind

## Technical requirements

All code examples for this chapter can be found on GitHub at `https://github.com/PacktPublishing/Democratizing-Artificial-Intelligence-with-UiPath/tree/main/Chapter08`.

Working with UiPath is very easy, and with the Community version, we can get started for free. However, for UiPath AI Center, we will need an enterprise license. You can acquire a 60-day enterprise trial license from `uipath.com`.

With the 60-day enterprise trial, we will have access to the following:

- Five RPA Developer Pro licenses – named user licenses include access to Studio, StudioX, Attended Robot, Apps, Action Center, and Task Capture
- Five unattended robots, five testing robots, and two AI robots
- AI Center, AI Computer Vision, Automation Hub, Data Service, Document Understanding, and Insights

For this chapter, you will require the following:

- UiPath Enterprise Cloud (with AI Center)
- UiPath Studio 2021.4+
- The `UiPath.IntelligentOCR.Activites` package v.4.13.2 or higher
- `UiPath.DocumentUnderstanding.ML.Activites` v1.7.0 or higher

> **Important Note**
> Directions on how to install UiPath packages can be found at https://docs.uipath.com/studio/docs/managing-activities-packages.

Check out the following video to see the Code In Action at: https://bit.ly/3J8gX53

## Enabling AI Center in the UiPath Enterprise trial

Additional technical requirements include leveraging AI Center. UiPath Document Understanding comes out of the box with the UiPath Community and Enterprise versions; however, with UiPath AI Center, we need to enable the service within the Enterprise trial. You can enable the service by following these steps:

- 1. Navigate to **Automation Cloud** at https://cloud.uipath.com:

![](img/05268897c8ac5d80eb3fead1cc8a822a_211_0.png)

### 2. Navigate to Admin:

![](img/05268897c8ac5d80eb3fead1cc8a822a_212_0.png)

Figure 8.2 – Click on Admin

### 3. In DefaultTenant, click on the three dots, and then click on Tenant Settings:

![](img/05268897c8ac5d80eb3fead1cc8a822a_212_1.png)

Figure 8.3 – Click on Tenant Settings

4. Choose **AI Center** under **Provision Services**, and click on **Save**:

![](img/05268897c8ac5d80eb3fead1cc8a822a_213_0.png)

Figure 8.4 – Check AI Center and then click Save

Once AI Center is enabled, we have completed the technical requirements for this chapter. In the next section, we will start gathering context about the use case by reviewing the current state.

## Understanding the current state

Before we jump into designing or building the use case, we need to understand the current state of the process. Every day, the Time and Expense (T&E) team of Company ABC needs to manually review receipts submitted from the business travels of their colleagues. As the T&E team reviews these receipts, they check them against their expense management tool to ensure that the employee submitted the expense report correctly. The following are the rules set by the T&E team (Figure 8.5):

![](img/05268897c8ac5d80eb3fead1cc8a822a_214_0.png)

Figure 8.5 – The high-level process flow

Reviewing the high-level process flow, the T&E team automatically receives expense reports once they are submitted by the employee. It is then up to the T&E analyst to open the report and check each expense and receipt for completeness and accuracy. If the report is accurate, the analyst can forward the report to the employee's manager for final approval.

Company ABC has a lot of employees that need to regularly travel, and the company has recently been hiring more employees, so the number of expense reports and receipts that the T&E team has to review is ever increasing. The T&E team has a turnaround time of 2 days, meaning they can review and approve an expense report 2 days after it has been submitted by an employee.

Looking at the current state, there appears to be plenty of opportunity for automation – reverting to the characteristics of an automation opportunity from Chapter 4, Identifying Cognitive Opportunities, we can see that this automation opportunity has the following characteristics:

- **Manual interactions with applications:** T&E analysts have to manually interact with the expense management tool.
- **Repetitive in nature:** T&E analysts have to perform the same series of steps for each expense and receipt.
- **Is time-consuming:** Reviewing individual receipts can be time-consuming for an analyst to perform.
- **Is rule-based and structured:** The rules surrounding expense reports are mostly structured.

The only area of concern with this process is with the receipts themselves, as receipts aren’t very structured and can vary from vendor to vendor. These points align with the characteristics of a cognitive automation opportunity also discussed in Chapter 4, Identifying Cognitive Opportunities:

- High variability
- Unstructured data

However, from a feasibility standpoint, we can easily create automation that handles receipts for the T&E analyst with UiPath Document Understanding. Also, from a viability standpoint, automating portions of this process for these analysts can provide the following benefits:

- **Increased throughput:** Implementing automation in the process can allow the analyst to complete reviews more quickly, reducing the 2-day turnaround time.
- **Increased capacity:** Implementing automation in the process can allow the analyst to complete more reviews, allowing the team to absorb the increased volume of expense reports without having to increase the team size or increase the turnaround time.
- **Error reduction:** Implementing automation can reduce errors made during manual review, minimizing the risk of approving an invalid expense.

Automation within this process can provide a great deal of impact for the T&E team. While there are many areas of opportunity, for this use case, we will focus on the receipt portion of the process. In the next section, we will focus on creating a future state design for implementing automation to process receipts.

## Creating the future state design

When we look at the high-level design of the current state, there looks to be several automation opportunities that we can create future state automation for (Figure 8.6):

![](img/05268897c8ac5d80eb3fead1cc8a822a_216_0.png)

Figure 8.6 – Automation opportunities with the expense report review process

It appears that most of the steps taken by the T&E team in the process could potentially benefit from automation, through a mixture of automation ideas that hand off information to each other. However, since we want to focus on Document Understanding with this use case exercise, let's only focus on the steps of the process that appear between Open Report and Send back to employee in case of error (Figure 8.7):
