As part of its Grant Application case type, FPAGM anticipates a number of activity types unique to this case type. It creates the following activity types in the system:

+   - Initial Inquiry/Notification
+   - Permission to Submit/Application Received
+   - Scope Definition
+   - Internal Draft Submitted for Review
+   - Final Internal Approval
+   - Grant Proposal Submitted
+   - Denied/Case Closed
+   - Accepted
+   - Thank You Letter
+   - Report

The list of case activity types, in conjunction with several general use activity types, should accommodate the various steps or decision results in the grant application process.

Each type may have an optional <max_instances> value which limits how many times the activity may be added to the case record:

```
<ActivityTypes>
  <ActivityType>
    <name>Open Case</name>
    <max_instances>1</max_instances>
  </ActivityType>
  <ActivityType>
    <name>Follow up</name>
  </ActivityType>
  <ActivityType>
    <name>Change Case Type</name>
  </ActivityType>
```

Note that the Open Case activity type has a max_instance of one. This makes sense as a case record can only be opened once, and thus this activity should only be present once in a single case.

When constructing your configuration file, make every effort to retain the indentation structure as found in the sample files and previous screenshot. Doing so will help you ensure that each tag closes properly and you don't get lost in the structural hierarchy.

As you define your activity types, you may want to have your configuration file open and ready for editing so that you can copy and paste the values from your browser to the XML file or vice-versa and thus ensure that the values match exactly.