The **Trigger Order** option allows you to prioritize the execution of record-triggered flows for a specific object. When saving a flow, you can set the **Trigger Order** number field to determine the order of execution. You can enter a number from 1 to 2,000, which dictates the order in which flows with the same object are triggered.

For example, you can prioritize an after-save flow to run before other after-save flows on the Account object. However, you cannot prioritize an after-save flow to run before any before-save flows or before an Apex trigger.

# Packaging the Trigger Order feature

The **Trigger Order** feature is fully packageable, which means it’s a best practice to leave space between your flow order numbering for customers to create their own flows and slot them in between your packaged flows if they want to. This flexibility allows customers to have more control over their automation processes.

# Customization with Flow Trigger Explorer

**Flow Trigger Explorer** is a unique feature to determine the order in which flow triggers run. If customers clone your templates or overridable flows, they can edit the Trigger Order on the clone, allowing them to further tailor the application to their specific needs.

# Best practices and limitations

When packaging flows, consider the following points:

+   - Each flow can have only 50² versions in an org, affecting both customers and developers. For customers, refer them to the help page on deleting a