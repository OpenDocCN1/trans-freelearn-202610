
## Practical scenario – failed flow example

Scenario: When the lead is updated to status = Working-Contacted and annual revenue is greater than $1 million, create a contact. We need to add a required field called security on contact via flow, and it should be 10 characters. We will hardcode this value in the flow for testing different variants.

Create a record-triggered flow with two conditions: status = Working-Contacted and annual revenue greater than 1 million.

This triggered flow runs immediately when the preceding conditions are met and a lead is updated. See Figure 8.8:

![](img/0c5ea2dc593e6f900cb4283ebfb6bf02_127_0.png)

Figure 8.8 – Record-triggered flow: Lead to Contact flow

Start the interview flow by clicking on Debug. This opens the Debug flow window; see Figure 8.9. The flow runs immediately and in rollback mode. This means that the flow debug process ensures that when the flow process ends, any operation on the database, such as create, update, or delete, is rolled back and not committed to the database. This is disabled by default, and we do have only a rollback option in debug mode. This ensures no data is saved to the database.

I added an existing lead, and the data from this lead will be utilized to run the flow (see Figure 8.9):

Set the Lead name as Hello World.

I created a contact with the same last name and email as the lead. This is to catch duplicate exceptions in the flow debug:

![](img/0c5ea2dc593e6f900cb4283ebfb6bf02_128_0.png)

Figure 8.9 – Lead to Contact: Debug flow window

After clicking on Run, the entry condition is met, and creating a record element in the flow is executed. As I said earlier, since there was a contact with the same last name and email, the flow errored as there was a duplicate contact record, and the execution ended. See Figure 8.10. We can see a NO icon next to the element where the flow failed:

Figure 8.10 – Flow failed due to a duplicate contact record

I fixed the duplicate contact issue and reran the flow for the same lead. But then I removed the hardcoded value for the security field, and this field is required to create a contact.

As discussed in Chapter 9, to create a new orchestration, click on a new flow orchestration from New Flow | All + Templates and select Start Record-Triggered Orchestration:

![](img/0c5ea2dc593e6f900cb4283ebfb6bf02_164_0.png)

Figure 10.5A – Partner onboarding – flow orchestration

- working 123, 124
- **flows**
  - considerations 44
  - designing tips 116, 117
  - versus flow orchestration 122
- **flows, Salesforce**
  - accessing 56, 57
  - autolaunched flow (no trigger) 56, 77
  - exploring 56
  - platform event-triggered flow 56, 68, 69
  - practical tips 80, 81
  - record-triggered flow 56, 61
  - schedule-triggered flow 66-68
  - schedule-triggered flow 56
  - screen flow 56-58
- **flows, to trigger business processes**
  - behind-the-scenes automated flows 41
  - interactive experience flows 41
- **FSD outlining tasks**
  - developing 19, 20
- **functional requirements** 16
  - examples 16
  - for automation, success tips 21
- **functional specification document (FSD)** 15
- **functional tasks, for automation**
  - identifying 17, 18

### I

- **input variables** 98
- **Interactive experience flows** 41, 42
  - approval processes 41
  - autolaunched flows 41
  - lightning components 42
  - screen flows 41
  - visualforce pages 42

### K

- **Know Your Customer (KYC)** 16

### L

- **lightning components** 42
- **Lightning Web Components (LWC)** 56
- **List View Controls (LVCs)** 25

### M

- **managed service providers (MSPs)** 8
- **marketing development funds (MDF)** 8
- **material non-public information (MNPI)** 42
- **minimum viable product (MVP)** 16, 31

### O

- **Order Of Execution (OOE)** 47-49
- **Organization-Wide Defaults (OWD)** 84

### P

- **partner registration** 140
  - process, scenario 140, 141
- **Partner Relationship Management (PRM)** 8
- **partner user onboarding process** 8-10
- **platform event-triggered flow** 42, 56, 68, 69
  - business scenario 69-71
  - working 72-76
- **potential automation steps**
  - identifying 26
- **practical scenario**
  - testing out 91-93
