![](bbox=[0.117, 0.096, 0.852, 0.399])

Figure 5.10 – UiPath Action Center

With UiPath Action Center, an unattended automation can send **actions** to Action Center that can then be assigned to specific humans to review. This allows for similar prompting provided from Validation Station or from input dialogs, just with Action Center storing the action for the human to interact with. Once the human interacts with their assigned action, an unattended automation can then pick up the user's input and continue with its task.

# Considerations for using unattended automation

With the scheduled nature of unattended automation come a few considerations around scheduling, documentation, and performance. Since unattended automations need a dedicated machine in order to perform automation jobs, there must be a machine available to run the automation, else the to-be performed automation must wait until a machine is freed up. If not planned correctly, this could lead to **service-level agreement (SLA)** issues with items not being completed in a timely manner. To resolve this, there could be a one-to-one mapping between automations and machines; that way, an automation always has a machine available when it's scheduled to run. Unfortunately, this could bring up issues with machine allocation and licensing, as additional machines and licenses will incur additional costs to the automation program. Automation programs should carefully balance automation schedules based on the machines they have provisioned.