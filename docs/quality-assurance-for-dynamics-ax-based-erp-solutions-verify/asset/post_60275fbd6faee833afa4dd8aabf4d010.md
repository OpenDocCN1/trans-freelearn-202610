
- All "Fix before breathing" and "Fix immediately" priority bugs are fixed and the fix is verified. Refer Chapter 8, *Defect Management System* for priority classification details.
- Severity level 1 and 2 bugs are fixed and the fix is verified.
- All issues or queries raised during module testing (related to functional behavior of requirement under testing) are settled.
- All test scripts related to requirements under test have been executed and screenshots of the results have been incorporated.
- If any medium or low-priority errors are pending, the implementation risk must be approved by the Business Analyst and/or Client.
- Completion criteria for all types of testing related to module testing stage (as mentioned in the table in the *Test Approach* section) have been met.

## Entry Criteria for System Testing

In order to enter the system testing stage it is necessary that all the customization work exit criteria for the module testing stage are passed and preparations for system testing have been completed. The preparation includes writing and reviewing of 'end-to-end' and 'scenario-based' functional and non-functional test cases. Hence, the following can be considered as the entry criteria for the system testing stage:

- Application has passed the exit criteria for module testing.
- Test cases or scripts that need to be executed in this stage are ready.
- All non-functional requirements are available:
  - Performance requirement
  - Security configurations
  - Countries and locales supported
  - Languages supported
- All non-functional test cases are ready as per the non-functional requirements.

## Exit Criteria for System Testing

After completing system testing all functional as well as non-functional requirements must be tested and important defects should not be in an open state. The system testing stage is also appropriate for the final security review of the application as it is the customized application in almost (except some possible issues discovered during regression testing) developed condition. So the following may be considered as the exit criteria for the system testing stage:

- All "Fix before breathing" and "Fix immediately" priority bugs are fixed and the fix is verified.
- Severity level 1 and 2 bugs are fixed and the fix is verified.
- If any medium or low-priority errors are pending, the implementation risk must be approved by the Business Analyst and/or Client.
- Test cases scheduled for the system test phase have passed.
- Final security review is completed and no security-related issues are in an open state.
- Completion criteria for all types of testing related to the system testing stage (as mentioned in the table in the *Test Approach* section) have been met.

## Entry Criteria for Regression Testing

Passing entry criteria for regression testing indicates that code has passed system testing and preparations for the regression testing (which include necessary test scripts and configurations of application) have been completed. Hence, the following may be the entry criteria for regression testing:

- Exit criteria for system testing have been met.
- Regression test scripts are ready.
- Security key or configuration keys are finalized.
- Consistency verification test script is ready.

> In order to optimize the efforts, the regression test scripts can be designed in a way that they can be used as consistency verification test scripts.

## Exit Criteria for Regression Testing

The following can be the exit criteria for the regression testing:

- All "Fix before breathing" and "Fix immediately" priority bugs are fixed and the fix is verified.
- Severity level 1 and 2 bugs are fixed and the fix is verified.
- If any medium or low-priority errors are pending, the implementation risk must be approved by the Business Analyst and/or Client.
- No open issues or queries exist.
- Completion criteria have been met for all types of testing related to the regression testing stage (as mentioned in the table in the *Test Approach* section)

## Entry Criteria for Release Testing

In the context of Dynamics AX the release testing stage has only installation testing. As per Microsoft standards installation and uninstallation processes should be safe and automated but some activities required for successful installation or uninstallation cannot be automated in Dynamics AX. For all such activities the manual procedure should be explained in the installation guide. The tester should follow all the steps mentioned in the installation guide and system behavior should be as mentioned in the installation guide. For more details about installation testing the reader can refer to the previous chapter. The following points can be considered as the entry criteria for the release testing stage:

- Successful completion of the exit criteria of the system testing stage.
- Installation guide is ready.
- Installation test cases are written and revised.
- The XPO or setup executable file that needs to be shipped is ready.
- The test environment for installation testing is ready.

## Exit Criteria for Release Testing

Release testing can be considered as completed after the following have been performed:

- All issues related to installation testing have been fixed.
- All open issues are documented as known bugs.
- Severity level 1 and 2 bugs are fixed and the fix is verified.
- If any medium or low-priority errors are pending, the implementation risk must be approved by the Business Analyst and/or Client.
- Completion criteria for all types of testing related to the release testing stage (as mentioned in the table in the Test Approach section) have been met.

## Criterion Definition

In order to provide clarity for the project testing team as well as the other teams in the project, it is essential that some of the key decision criteria are documented in a test plan and a commitment is taken from various stakeholders.

## Test Case Pass or Fail Criteria

Every failed test case (except at unit testing) will be recorded. The application will not move to the subsequent software testing stage with Severity level 1 or 2 or urgent or high priority errors. Following may be a generalized idea about the test case pass fail criteria. More accurate definition can be defined for each type of individual test case type (i.e. testing types):

- The feature will pass or fail depending upon the results of testing actions. If the actual output from an action is the same as the expected output specified by a test case, the feature passes. Should any action within a test case fail, the entire case fails.
- If a test case fails, it is not assumed that the code is defective. A failure can only be interpreted as a difference between the expected results, which are derived from project documentation, and actual results. There is always the possibility that expected results can be in error because of misinterpreted, incomplete, or inaccurate project documentation. Reported failure is logged for further analysis and then the cause of failure is analyzed. If it is a defect, it should be recorded on the review and authorizations form.

## Suspension/Resumption Criteria

The test team may suspend partial or full-testing activities on a given piece of code if any of the following occurs:

- Test environment is not updated with the latest code (which needs to be tested).
- The development team cannot configure the setup data for a given piece of code.
- Sanity (or build verification) testing (if any is being conducted) fails.
- There is a fault with a feature that prevents its testing.
- An item does not contain the specified changes.
- An excessive amount of bugs that should have been caught during the previous phase are found during more advanced phases of testing. Here it is very much imperative that the criteria to define excessive bug are defined i.e. X number of defects with severity 1 or 2 or Y number of defects with priority critical or urgent.
- A new version of the software is available to test.
