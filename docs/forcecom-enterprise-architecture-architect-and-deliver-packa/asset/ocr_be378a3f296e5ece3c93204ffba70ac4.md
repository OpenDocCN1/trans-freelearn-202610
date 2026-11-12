For example, try changing the method name of one of the global Apex methods (for example, ComplianceService.report to ComplianceService.reportX) as well as the methods in ComplianceResource and ComplianceResource_1_0 calling it. While this compiles in the developer and build orgs, it eventually results in the following error from the Jenkins build, as the check deployment to the packaging org failed because published global methods cannot be removed or renamed. In this case, the following build failure will be reported by Jenkins:

```
classes/ComplianceService.cls -- Error: Global/WebService identifiers cannot be removed from managed application: Method: LIST<fforce>. ComplianceService.VerifyResult> report(SET<Id>) (line 1, column 8)
```

The dashboard on Jenkins shows the project with a dark cloud as one out of the last two builds have now failed. It is time to fix the build:

![](bbox=[0.11, 0.33, 0.91, 0.5])

Once a new build completes successfully, Jenkins rewards you with some sunshine:

![](bbox=[0.11, 0.56, 0.91, 0.71])