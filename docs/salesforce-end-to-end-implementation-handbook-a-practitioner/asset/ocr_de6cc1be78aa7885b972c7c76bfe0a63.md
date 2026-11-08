There may be many reasons why and instances in which you will need to keep two systems active. Whatever the reason, concurrency may have the following impacts, depending on the nature of your Salesforce solution and roll-out planning:

+   - Users on both systems at the same time means they will be update and entering new data into both systems and it will be out of sync:
+     - This can be mitigated by doing regular delta data migrations or (if a viable economic option) creating interim integrations between the two systems until the old system can be retired:
+       - The old system sometimes is left available but in a read-only state so users can reference legacy data if needed. If that approach is taken, it should be limited to a short period after go-live.
+   - Another consideration is the feasibility of integrations with third-party systems:
+     - Can the new Salesforce system and legacy CRM both be integrated with other systems at the same time?
+     - If not, choosing whether Salesforce for your legacy CRM should be connected to the third-party system is an important decision to be made.

In Figure 10.3, you can see an example of how our scenario company, PME, has chosen to phase the roll-out of their Salesforce solution: