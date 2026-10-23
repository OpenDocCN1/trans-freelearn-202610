
The preceding flowchart shows that the process begins with the **OT administrator** (the one responsible for OT in a plant/organization). It is the OT administrator's responsibility to initiate the process and direct each technology owner in the OT environment to begin their tasks and report back. The recommendations summary that's prepared at the end of the process is to be kept by the OT administrator and used during budgeting and resource planning activities:

- **Incident response preparation inputs:**
  - OT security program policies, processes, and procedures
  - Asset inventory
  - Completed incident response forms
- **Incident response preparation outputs:**
  - Modified incident response policy, processes, and procedures
  - Recommendations summary document

Next, let's discuss the other side of an IR plan: the incident handling process.

## Incident handling process

The goal of the incident handling process is to provide a framework that aids the OT security team in handling incidents efficiently and effectively. By predefining a process with actions to be carried out by role holders, the chaos surrounding an incident can be managed with a systematic response. If each role holder understands and practices their responsibilities according to the plan, actions and communications are streamlined.

By its nature, the incident handling process is initiated when a suspected incident is detected, whether by an employee, a contractor, or a vendor.

All changes to the OT environment that are made as part of the incident handling process should be made in compliance with the **Management of Change (MOC)** policy.

## Incident handling process flow

The following process flow diagram describes the major steps of the incident handling process. This process and its accompanying procedures assist the organization in handling current and future incidents:

![](img/c8ffa9959721097701584fcc95101362_720_0.png)

Figure 18.2 – Incident handling process flow

The preceding flowchart shows that the process begins with detecting a suspected incident by any employee, contractor, or vendor, who is referred to as the **incident reporter**. The incident reporter reports the incident to the OT administrator, or designated person, by email, phone call, text message, or in person. The OT administrator then makes a judgment as to whether the incident is in scope and should be handled by the OT staff according to this process, or whether it is to be handled by the **Information Technology (IT)** staff and their cyber incident response team with safety/security involvement.

If the suspected incident is deemed in scope for this process, the OT administrator immediately alerts the relevant technology owner(s) and assigns an **incident lead** (the person who will see the incident handling process through to satisfactory completion).

The technology owner(s) immediately begins short-term containment and analysis, while the incident lead begins their role as the focal point for information status and coordinating efforts.

The Incident Lead initiates the Incident Response Form (covered in the *Example incident report form section*) with information gathered from the incident reporter, OT administrator, and technology owner(s). The Incident Lead then leads the discussion with all the involved parties to decide on the severity of the incident. If a disagreement occurs, the OT Administrator has the final say.

If the incident's severity is determined to be Not an Incident, the Incident Lead can proceed directly to completing the Incident Response Form and communicating the closed status.

Otherwise, for low-, medium-, or high-severity incidents, the Technology Owner(s) begins the procedures for long-term containment, followed by eradication, then recovery. The Incident Lead continues to gather information to complete the Incident Response Form while coordinating appropriate communications per the communications procedure.

If the incident's severity is determined to be *high*, in addition to the aforementioned actions, the OT Administrator involves the (IT) cyber incident response team and safety/security representatives. Also, the communications plan will call for the involvement of senior leadership and legal counsel, to decide what other escalation measures are appropriate.

Once recovery is thought to be achieved, there is a watchful period where it is determined whether symptoms remain. If incident symptoms are detected, the containment/eradication/recovery path is determined not to be entirely successful and a new round of analysis, short-term containment, severity determination, and containment/eradication/recovery is undertaken.

Once the containment/eradication/recovery path is found to be successful, the involved parties gather for a lessons learned exercise while the incident is still fresh in their minds. The Incident Lead facilitates the discussion and documents the findings on the Incident Response Form. Afterward, there is a period for incident closure, where Technology Owners verify that systems operations have returned to normal, including removing short-term containment measures, as appropriate.

During the final step, the Incident Response Form is completed by the Incident Lead and submitted to the OT Administrator. Then, an incident closed status is communicated:

- **Incident handling inputs:**
  - Blank incident response form
- **Incident handling outputs:**
  - Completed incident response form.
  - Normally operating OT environment.
  - Closed incident status is communicated.

Next, we are going to discuss incident response procedures.

## Incident response procedures

The SANS Institute describes a six-step incident handling process, as shown in the following diagram. The first step, **Preparation**, corresponds to incident response preparation processes and procedures, while the other five steps (**Identification**, **Containment**, **Eradication**, **Recovery**, and **Lessons Learned**) correspond to incident handling processes and procedures:

![](img/c8ffa9959721097701584fcc95101362_722_0.png)

The following procedures provide more details about how the steps in each of the process flows are to be accomplished. Because of the diverse nature of incidents, the procedures do not attempt to give step-by-step instructions but rather focus on a methodology to organize effort, manage resources, and communicate about incidents to stakeholders.

## Incident response preparation process

As shown in the following diagram, all the tasks of the incident response preparation process shown in green are mapped to the preparation step of the SANS Institute six-step incident handling process:

![](img/c8ffa9959721097701584fcc95101362_723_0.png)

Figure 18.4 – Incident response preparation process, mapped to the SANS preparation step

The tasks are divided between the OT Administrator and the Technology Owner roles. It is the OT Administrator's responsibility to start, end, and manage the process. Each step will be described in more detail in the procedures that follow.

## Reviewing OT security program maturity

The best preparation we can do to handle cybersecurity incidents is to prevent them from occurring in the first place. A properly defined OT security program has been created for this purpose. As a matter of fact, a best practice is to evaluate the state and effectiveness of the OT security program during the periodic incident response preparation process. If a separate periodic OT security program review has been conducted recently, its results should be examined and augmented with additional detail as appropriate.

At a minimum, the following should be surveyed:

- **Governance:**
  - Do members of the OT team understand what their OT security program roles and responsibilities are?
- **Training:**
  - Have OT security program role members been trained on the policies, processes, and procedures? Has there been a refresher that highlights any recent changes?
  - Do OT security program role members have training plans for developing required skills and competencies?
  - Has everyone who connects to the OT environment completed security awareness training?
- **Information classification:**
  - Is information regularly classified for appropriate handling (for example, confidential versus unrestricted) by personnel, contractors, and vendors?
  - Has data and configurations been wiped from any asset that has been retired from the OT environment?
- **Management of Change (MoC):**
  - Is the MoC process followed for changes to OT assets?
  - After reviewing the MoC forms from the previous year, does the percentage of emergency changes seem appropriate? Has there been a change in the percentage of emergency changes since the last review?
  - Are there known changes that have been made to the OT environment that did not go through an MoC process but should have?
- **Asset inventory:**
  - Does the asset inventory seem complete? Has it been updated for newly added or removed assets? Has a **Recovery Time Objective (RTO)** and **Recovery Point Objective (RPO)** been assigned for all assets? Has an individual been identified as the Technology Owner (or equivalent) for each asset?
  - Does the asset inventory seem accurate? Has the asset inventory been updated for changes in the OT environment? Has the asset inventory been verified to represent the current state?
