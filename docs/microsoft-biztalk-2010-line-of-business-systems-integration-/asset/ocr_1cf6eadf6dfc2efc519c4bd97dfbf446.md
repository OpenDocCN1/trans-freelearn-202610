![](bbox=[0.415, 0.148, 0.585, 0.389])

+   5. Click the **New Rule** button to start the workflow wizard and then select the **Account** object as the entity that this workflow acts upon.
+   6. Next, name the rule **BillingStreetChanged** and set the **Evaluation Criteria** to run **Every time a record is created or edited**.
+   7. Set the rule to run if the formula evaluates to true and set `ISCHANGED(BillingStreet)` as the formula. This formula returns true if the field has a different value than it did before. Save this configuration and proceed.
+   8. Specify two workflow actions. The first action adds a unique token to the Account object and the second workflow action actually sends the outbound message to BizTalk Server through the Azure AppFabric Service Bus.
+   9. Set the first workflow action to **New Field Update**.

![](bbox=[0.423, 0.634, 0.577, 0.784])