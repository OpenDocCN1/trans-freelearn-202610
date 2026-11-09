be good enough to be released to the internal customer i.e. the test team. The exit criteria for unit testing is as follows:

+   - Unit test has been passed.
+   - The code is complete with respect to requirements under test i.e. there are no missing features or elements.
+   - Code documentation is complete i.e. both comments and the job description.
+   - Dynamics AX best practices errors and warnings (as far as possible) have been removed as per the test approach section. This is an optional criterion as sometimes this testing is performed by the test team under module testing.
+   - Code optimization for three-tier architecture is done and the best practice tool is not showing any error related to performance optimization of the code.
+   - Creation of all label files and removal of unused label files including all info logs, warnings, etc.
+   - Unused, unreachable, and redundant code is removed.
+   - Declaration of successful unit test performance is signed/recorded

# Entry Criteria for Module Testing

As discussed earlier, the exit criteria for one testing stage may not be the entry criteria for another. For example, module testing cannot be started immediately after unit testing is finished (i.e. exit criteria for unit testing are passed); the code that has passed the unit test needs to be deployed to the test environment and only then can module testing start. So the following can be considered as the entry criteria for module testing:

+   - All required units have passed the unit test exit criteria.
+   - The unit test document is signed by the developer or peer of the developer who conducted unit testing.
+   - The code is deployed in the test environment (having different test environments is the ideal approach).

# Exit Criteria for Module Testing

The objective of module testing is to complete the various testing for this stage as per the latest understanding of the requirements. Hence, it is necessary that the completion criteria for all testing types are finished before completing this testing stage, and if there is any change in understanding the requirements (through query or issue registers requests), the impact of change should be studied, analyzed, and tested before exiting this stage. It is also imperative that all important issues or defects raised are closed at any terminating stage so that no future tracking is required for any issue or defect. So the following can be considered as the exit criteria for the module testing stage: