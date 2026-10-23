
## Evaluation Statistics

| Metric | Value |
|---|---|
| accuracy | 0.53353 |
| precision | 0.52848 |
| recall | 0.53353 |
| matthews correlation | 0.41685 |
| fscore | 0.5275 |
| auroc | 0.8411 |

Figure 7.8 – Example evaluation report

Figure 7.8 is an example of an evaluation of a text classification model. We generally want to aim for a greater-than-70% Fscore. In this case, we're lower than 70%, so we should review our training and evaluation datasets for correctness and investigate editing model parameters or choosing a different model altogether.

Once we choose a model and successfully evaluate it, we can move into testing the model with the testing dataset. This dataset should be completely unbiased and full of new data the model hasn't seen before. The best type of data for testing is young data, preferably coming from our production environment.

## Executing UAT

In previous sections, we discussed testing RPA and ML in their own, distinct silos. This works well during the development phases of the automation life cycle, but there should be a convergence of the two technologies before deployment into production. This convergence should occur during the UAT phase of the automation life cycle.

UAT is a type of testing performed to get final approval by the end users or businesses on the performance of the automation. This testing is performed prior to deploying the automation into production, showing the end user how the automation should perform given a set of scenarios. During this testing, we do not focus on specific errors such as selectors but focus on the E2E process flow, ensuring that the automation's output meets the requirements of the end user/business.

The following diagram depicts how UAT fits into the automation life cycle:

![](img/05268897c8ac5d80eb3fead1cc8a822a_202_0.png)

Figure 7.9 – UAT within the automation life cycle

For us to execute UAT, there are a few entry points to consider as criteria prior to final testing. These entry criteria include the following:

- **Development testing completed**: Automation development should be tested (unit, integration, and system tested) and should have run through E2E testing prior to acceptance testing.
- **Peer review completed**: Automation development should be peer-reviewed by another developer for best practice.
- **Environment prepared**: UAT generally occurs in a separate environment from development. This environment can include UAT applications with testing data and a UAT UiRobot.
- **Automation migrated**: Automation should be published into the development orchestrator, committed to source control, and migrated into the UAT orchestrator prior to UAT testing.
- **UAT test plan created**: A test plan with all necessary test scenarios and expected outcomes should be created by the process owner or **subject-matter expert (SME)**.
- **UAT data gathered**: In conjunction with the test plan, data for UAT should be gathered to test with. Generally, recent production data is best to perform UAT with.

For more information on UAT, you can refer to the following link:

```
https://www.guru99.com/user-acceptance-testing.html
```

Once all the entry criteria are completed, UAT testing can commence. Executing UAT can take a few forms, depending on whether the business runs the UAT tests or the development team runs the UAT tests for the business to review. An easy way to conduct UAT is to have the SME and development team in a meeting together to run through each test scenario. The SME can supervise the test, while the developer can launch the automation from the UAT orchestrator for each test scenario.

It is important to have exit criteria defined for the completion of UAT. Exit criteria are best defined within a UAT test plan document so that all parties involved know the required criteria for UAT completion. You can see an example of a UAT test plan document in the following screenshot:

| # | Scenario | Expected outcome | Case ID | Status |
|---|---|---|---|---|
| 1 | - Market Data Rejects > 5% of total Integrated<br>- Market_Data_Bypass is not true | The robot:<br>- References Market Data portion of output email<br>- Sends Email:<br>  - To: team@company.com<br>  - Subject: UIP Alert: Market Data Rejects Over 5%<br>  - Body: Today's Kyriba Status Report shows a high amount of Rejections.<br>- END PROCESS | 1 | APPROVED |
| 2 | - Market Data Rejects > 5% of total Integrated<br>- Market_Data_Bypass is true | The robot:<br>- References Market Data portion of output email<br>- All other functions as expected; formats and distributes email as expected | 2 | APPROVED |
| 3 | - Mark to Market Report Missing | The robot:<br>- References Market Data portion of output email<br>- Filters for 'mtm' template<br>- Runs MTM Reporting using RPA _ FX MTM Generation template<br>- All other functions as expected; formats and distributes email as expected | 3 | APPROVED |
| 4 | - Mark to Market Report is 'Warning' | The robot:<br>- References Market Data portion of output email<br>- Filters for 'mtm' template<br>- Sends Email:<br>  - To: team@company.com<br>  - Subject: UIP Alert Check MTM Status<br>  - Body: 'Mark to Market status is: Warning - please investigate'<br>- END PROCESS | 4 | APPROVED |
| 5 | - Mark to Market Report is 'Error' | The robot:<br>- References Market Data portion of output email<br>- Filters for 'mtm' template<br>- Sends Email:<br>  - To: team@company.com<br>  - Subject: UIP Alert Check MTM Status<br>  - Body: 'Mark to Market status is: Error - please investigate'<br>- END PROCESS | 5 | APPROVED |

![](img/05268897c8ac5d80eb3fead1cc8a822a_203_0.png)

Exit criteria can depend on the organization, but generally, UAT exit criteria include the following:

- No defects open
- Successful completion of UAT test scenarios
- Documented sign-off of UAT

To meet these exit criteria, the business analyst, UAT testers, or SME should be documenting the result of each test scenario completed. Depending on the organization, each test scenario can be documented through a screenshot, video recording, or just sign-off on the automation output. Once the exit criteria are met, UAT can be fully completed, and the automation can be deployed into production.

## Closing the feedback loop

As cognitive automation is moved from UAT into production, it can face data not seen from the training, evaluation, and testing test sets. We expect the deployed model to be able to handle new data based on the training performed, but there may be times where the model returns an unsatisfactory result or there are opportunities to further improve the model's performance based on the data it encounters.

This is where closing the feedback loop can play a large factor in the performance of an ML model. By closing the feedback loop on a Document Understanding or AI Center ML skill, we can capture unseen data points, using a human to send feedback to the ML skill and continuously train the skill with new data. You can see a representation of closing the feedback loop in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_204_0.png)

Figure 7.11 – Closing the feedback loop

With UiPath, developers can use a confidence threshold to allow automation to continue with the ML skill's output or prompt users for input by using Attended Automation (Validation Station), or by using Action Center. This feedback loop from human input now allows us to continuously test and refine our model with fresh, relevant data.

## Section 3: Building with UiPath Document Understanding, AI Center, and Druid

In this section, we will build practical use cases using UiPath Document Understanding, AI Center, and Druid connections to tie UiPath Automation to cognitive abilities.

This section comprises the following chapters:

- Chapter 8, Use Case 1 – Receipt Processing with Document Understanding
- Chapter 9, Use Case 2 – Email Classification with AI Center
- Chapter 10, Use Case 3 – Chatbots with Druid
- Chapter 11, AI Center Advanced Topics
