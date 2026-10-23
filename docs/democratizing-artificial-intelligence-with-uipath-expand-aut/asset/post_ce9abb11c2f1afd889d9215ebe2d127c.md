
Because RPA doesn't deviate much from traditional programming, we can leverage traditional software development testing practices when testing automation. In software testing, there are many different types of testing leveraged to ensure code is performing as desired, as illustrated in the following diagram:

![](img/05268897c8ac5d80eb3fead1cc8a822a_194_0.png)

Figure 7.2 – Hierarchy of development testing

The following list covers the hierarchy in brief:

- **Unit Testing:** Lowest level of testing—tests individual code components or functions used by software
- **Integration Testing:** Verifies different modules or components work together
- **Functional Testing:** Verifies business requirements, such as verifying the output of an action or logic is as expected
- **End-to-End Testing:** Replicates user behavior and verifies the E2E flow is running as expected
- **Acceptance Testing:** Series of formal tests to ensure development matches business requirements

For more information on the types of software testing, you can refer to the following link:

https://www.atlassian.com/continous-delivery/software-testing/types-of-software-testing

While automation programs may have different levels of formal testing required during their automation life cycles, all development must undergo some levels of formal acceptance testing. This leaves the lower levels of testing (unit to E2E) to the development team to perform prior to UAT.

## How to test cognitive components

When testing a cognitive component, all testing starts with our data. In Chapter 6, Understanding Your Tools, we completed an AI Center example with a training and evaluation dataset and pipeline. However, as we approach deployment of an ML skill, we need to add another dataset—the training dataset, leaving us with the following datasets necessary when building cognitive automation:

![](img/05268897c8ac5d80eb3fead1cc8a822a_195_0.png)

Figure 7.3 – Testing an ML skill

These datasets are outlined in more detail here:

- **Training dataset**: Initial dataset to train the ML model. This training set should simulate our ideal expected result—or *ground truth*, in other words.
- **Evaluation dataset**: A smaller set of data used to evaluate the training of the ML model.
- **Testing dataset**: An unbiased dataset used to expose the trained ML model to real-life data.

**Important Note**
For more information about *ground truth*, check out the following link:
https://towardsdatascience.com/in-ai-the-objective-is-subjective-4614795d179b

In summary, we can see that RPA and cognitive components require different types of testing. RPA requires more of a traditional **software development life cycle (SDLC)** testing approach, while cognitive automation requires an iterative, data-driven approach. While respecting the mindset of **SoC**, we can effectively silo development and testing of both RPA and cognitive automation to ensure both components work correctly before bridging them together for UAT. In the next section, we will venture into executing cognitive automation testing.

## Executing cognitive automation testing

For successful testing and deployment of cognitive automation, we should always prepare an approach to testing. In this section, let’s review details on gathering test data, executing RPA and cognitive tests, and tying it all together with executing UAT tests.

### Gathering test data

ML depends highly on data. Having the right types and the right amount of data is crucial in having a successful ML model deployed; therefore, data preparation is such an important part of the ML process.

When gathering test data, many organizations ask how much data is necessary to get started. Unfortunately, there isn’t an explicit answer to this question, as there are many variables that can affect how much data is necessary, such as the following:

- The complexity of the business problem ML must solve
- The number of classifications (if necessary)
- The complexity of the algorithm used

If necessary, you can try to target the following estimations when gathering data. Of course, every problem may be slightly different, thus more data may be required to output satisfactory results:

- **Text classification**: Try to have a couple of hundred examples of *EACH* classification.
- **Tree-based Pipeline Optimization Tool (TPOT) tabular classification**: Again, a few hundred examples of each classification.

In general, when trying to gather data from a business team, you want to strive for as many data points as possible. It’s always easier to have too much data and to cut down, versus going back to the business and requesting more data.

After you gather the data necessary for the project, it is important to have enough data to train, validate, and test the ML model, as shown in the following screenshot:

![](img/05268897c8ac5d80eb3fead1cc8a822a_197_0.png)

Figure 7.4 – Splitting our dataset

A majority of our dataset should be used for training the ML model, while smaller datasets can be used to validate the model and to finally test the model. Here’s an estimated rule of thumb:

- **Training Dataset**: 60-65%
- **Validation Dataset**: 20-25%
- **Testing Dataset**: 10-20%

Once our data for executing testing is gathered, we can continue on to the next section of cognitive automation testing: execution.

### Executing RPA testing

As we test RPA, we should start with the hierarchy of development testing (Figure 7.2) from earlier in the chapter. In order to easily enable this hierarchy, there are a few practices we should follow during development, as outlined here:

- **Small workflows**: Workflows should have a distinct input and output, performing a set action (such as Log in to X, Navigate to Y, Read Z).
- **Independent workflows**: A workflow (minus Main.xaml or Process.xaml) should not call other workflows.

By creating smaller, modular workflows, we can more easily create unit tests to test the functionality of development.

When creating test cases for unit testing, we want to follow a methodology that allows us to prepare our environment to run the workflow, test our workflow, and validate the workflow's output. The methodology is known as the *Given-When-Then* formula of testing and is elaborated on here:

- **Given** some context
- **When** some action is carried out
- **Then** some output should be obtained

For more information on *Given-When-Then*, you can refer to the following link:

https://www.agilealliance.org/glossary/gwt/

As test cases are built out in UiPath, separate sequences are used to separate the *Given*, *When* and *Then* portions of the *Given-When-Then* formula, as illustrated in the following diagram:

![](img/05268897c8ac5d80eb3fead1cc8a822a_198_0.png)

Figure 7.5 – Creating a test case

With REFramework's `Test_Framework` folder, we can run each test case created using the `Run_Tests.xaml` file, outputting the results of our test into an Excel and TXT log file, as shown in the following screenshot.

| WorkflowFile | Exception | Status | Comments |
|---|---|---|---|
| Framework\\InitAllSettings.xaml | Success | PASS | |
| Framework\\InitAllSettings.xaml | Success | PASS | |
| Test_Framework\\ACME\\Basic Actions\\_Test_ACME_Open.xaml | Success | PASS | AppEx: Account not found: John_Doe@xyz.com at Source: ACME |
| Test_Framework\\ACME\\Basic Actions\\_Test_ACME_Login.xaml | Success | FAIL | Login: Send Outlook Mail Message |
| Test_Framework\\ACME\\Basic Actions\\_Test_ACME_Logout.xaml | Success | PASS | |
| Test_Framework\\ACME\\Basic Actions\\_Test_ACME_Close.xaml | Success | PASS | |
| Test_Framework\\SHA1\\Main Page\\_Test_SHA1_Read_Hash.xaml | Success | PASS | |
| Test_Framework\\SHA1\\Main Page\\_Test_SHA1_Enter_Data.xaml | Success | PASS | |

![](img/05268897c8ac5d80eb3fead1cc8a822a_200_0.png)

REFramework's testing framework is just one method of testing our development. Other methods include leveraging UiPath's Test Suite to run and report on our test cases. Either way, by following this *Given-When-Then* type framework, we have a method to create test cases for each workflow that we can use for development testing and regression testing post-deployment.

### Executing cognitive testing

As we test an ML skill, we should start with the training dataset, building a training pipeline to train the model with an initial set of data. Once trained, the model can be evaluated with the evaluation (or sometimes called validation) dataset. Once our dataset is evaluated, we can review the model's evaluation report to judge the model's performance. This evaluation report is usually a direct output of an evaluation pipeline, as illustrated in the following screenshot:
