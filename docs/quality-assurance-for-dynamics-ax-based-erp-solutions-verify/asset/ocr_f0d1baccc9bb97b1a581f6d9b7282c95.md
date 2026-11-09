# Test Case Pass or Fail Criteria

Every failed test case (except at unit testing) will be recorded. The application will not move to the subsequent software testing stage with Severity level 1 or 2 or urgent or high priority errors. Following may be a generalized idea about the test case pass fail criteria. More accurate definition can be defined for each type of individual test case type (i.e. testing types):

+   - The feature will pass or fail depending upon the results of testing actions. If the actual output from an action is the same as the expected output specified by a test case, the feature passes. Should any action within a test case fail, the entire case fails.
+   - If a test case fails, it is not assumed that the code is defective. A failure can only be interpreted as a difference between the expected results, which are derived from project documentation, and actual results. There is always the possibility that expected results can be in error because of misinterpreted, incomplete, or inaccurate project documentation. Reported failure is logged for further analysis and then the cause of failure is analyzed. If it is a defect, it should be recorded on the review and authorizations form.

# Suspension/Resumption Criteria

The test team may suspend partial or full-testing activities on a given piece of code if any of the following occurs:

+   - Test environment is not updated with the latest code (which needs to be tested).
+   - The development team cannot configure the setup data for a given piece of code.
+   - Sanity (or build verification) testing (if any is being conducted) fails.
+   - There is a fault with a feature that prevents its testing.
+   - An item does not contain the specified changes.
+   - An excessive amount of bugs that should have been caught during the previous phase are found during more advanced phases of testing. Here it is very much imperative that the criteria to define excessive bug are defined i.e. X number of defects with severity 1 or 2 or Y number of defects with priority critical or urgent.
+   - A new version of the software is available to test.