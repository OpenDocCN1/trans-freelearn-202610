
| Field | Description |
| --- | --- |
| FSD ID | You can use any unique identifier; the best practice is to concatenate FS, BRD ID, and NN since one BRD can have multiple FSD line items – for example: FS- Account-01.01, FS- Account-01, 02..., FS- Account-01.10. |
| FSD summary | Summary of requirements in more technical terms so that the technical team understands exactly the steps in the business requirements. |
| FSD details | It includes granular details that your technical team can understand. Process flows will help you capture these details more accurately. |
| Normal flow | This is the most optimistic flow. This is what the user does to achieve their goal when no issues are encountered – a happy path. |
| Alternative flow | If the user encounters issues, provide steps for them to take a different flow. |
| Exception flow | In the event of an error, this will be the system response. |
| Business rule | Capture business rules associated with a requirement. For example, account records cannot be converted from Prospect to Customer without a KYC ID. |
| User actions | The steps the users perform in the system – for example, logging in to Salesforce, accessing the Account tab, clicking on Create new records, entering all the required fields, and successfully saving the record. |
| System response | The system displays a human-understandable success, error, or information message. |
| Complexity (H/M/L) | High, medium, or low. |
| Can be automated? | Yes/no. |
| Estimated effort | Capture estimates with and without automation. This will give us an idea if we should or should not automate this step of the functional specification. |
| Phase of project | Phases such as Release -1, Release -2, Release -3, and so on. |
| Documented date | The date when a requirement is added to a matrix. |
| In/out scope | Your prioritized list usually consists of approved in-scope items; in some cases, due to technical limitations or resource constraints, you can defer a few requirements to future releases. Here, you can tag the deferred ones as out of scope. |
| Comments | Add any comments or details that add clarity to your requirement. |

This is a comprehensive list of key sections in functional documents. As needed, add other relevant sections that you think add value to your implementation. As with BRD, you review this with technical and testing leads and SMEs, get agreement and approval from all, and lock this document. You should share this document with business SMEs and stakeholders so that they can review and provide input. However, their approval may be optional. This baseline version will be your starting document for subsequent releases on your roadmap.

## Scenarios – Rationale for automation

We will explore a real-world scenario in this section, starting with defining high-level requirements and going more granularly into low-level requirements. With the aid of a process flow, we will discuss the rationale behind the steps we plan to automate.

### High-level requirement:

The user will be able to request and gain access to accounts and related records such as contacts, opportunities, and cases. Users not within specific business groups need compliance approval before they can be granted access to the account and related records.

### Low-level functional requirements:

1. The sales analyst requests access to Salesforce data.
2. The sales manager/sales operations will be able to add users to the account team.
3. Compliance needs to approve user access if they do not belong to the same group. Example: Sales users in Zone 1 can be granted access to any of the records owned by users from Zone 1, whereas if a Zone 2 user needs access to Zone 1 users’ data, compliance approval is required.
4. Compliance reviews the request and can approve or reject the case.

Let’s go through each step and see what can be automated and see if that makes sense:

1. Can the sales analyst request access directly from the system, by creating a case?
2. If a request conforms to the guidelines (which means users belong to the same sales group), can we automate account team creation on case approval by the sales manager/operations, or do they need to create an account team manually? This makes sense to automate, but do we have time and resources? What is the volume of users we have in our instance? If not a large volume, does it make sense to automate?
3. Is case management required? Or do we get approval outside the system?
4. Do we need to build approval processes if the data volume and frequency are low? Do we have licenses to the system for compliance users? Assuming you have good user bases and medium to high volume data, it makes sense to enable case management and automate account team creation. Here is the reason: We need cases available handy for auditing and automating account team creation to help compliance users.
5. Do we automate email notifications based on actions in the system, or is it done outside the system? Do the users need to be active in the system to be able to get notifications? Automating email notifications sounds like a no-brainer, but what if the users do not have access to the system and you need a response from the user, such as approve/reject?

> **Note**
> If the same set of users needs similar access to data, the account owner will be able to add the user to the account team and sales team from their user record. This is a one-time process, and we can default on this team on all account teams and opportunity teams. If they are a different set of users, team members need to be added at the account/opportunity record level.

Now that we have identified the requirement in more detail and are able to assess if it adds value to automate all or some steps in the process, the next very important step is to socialize with all our key stakeholders and the project team and get a common understanding and agreement on the requirements as a whole, as well as potential tasks planned to be automated. Let’s check this out in the next section.

## Getting consensus and agreement from all stakeholders

In the last section, we got an opportunity to identify all the requirement steps – most importantly, the ones that can potentially be automated using a process flow diagram. I cannot emphasize enough how important these process flows are. If your team cannot draw your business process and explain it visually, that means you do not understand your business process completely, and this translates to incomplete requirements gathering and understanding of your business needs. If this is the case, take a pause and make sure requirements are elicited, documented, and agreed on by all project stakeholders. Make use of a whiteboard or a flip chart during your workshops, and develop the process flow with the help of your business stakeholders, SMEs, and project team members.

Here are some considerations to make an agreement with key stakeholders:

- **Be an active listener**: Be able to understand the meaning behind the work and the user’s intentions.
- Understand the user needs in the context and be technology agnostic.
- Avoid stakeholders telling you what to do (solution) rather than what they need.
- Adopt incremental approaches where possible to foster innovative solutions and better solutions.
- Take notes… lots of them. Tag the source where you got the information. Later, if you need clarification, it helps you identify the source easily.
- Use visuals as much as possible to bring everyone to a common understanding.
- **Communicate**: Send the minutes to all stakeholders, including the visuals, and solicit feedback.

You can make prioritization meetings meaningful and effective, as follows:

- Engagement rules should be communicated
- Prioritization meetings should be well planned and communicated
- A clear agenda and goals should be communicated
- Ground rules for conflict resolution should be established
