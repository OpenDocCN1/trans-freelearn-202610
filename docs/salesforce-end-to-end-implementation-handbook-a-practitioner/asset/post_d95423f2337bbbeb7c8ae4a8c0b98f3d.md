
exercises.

- **Scribe**: Someone should take notes throughout the workshop – at least in the group sessions. If you have two people facilitating, one could be a scribe if you are not able to have a dedicated scribe and timekeeper resource.
- **Timekeeper**: Make sure you stay on track and are able to complete the planned agenda by assigning a timekeeper to state when there are 15, 10, and 2 minutes left in each exercise.
- **Workshop products**: The output of the workshop are the workshop products. These are the gems of knowledge you have been seeking to uncover by having the workshop. Here is a brief description of key workshop products you should seek to produce:
  - **Actors**: Identified actors (end user groups) involved in the current business process.
  - **Processes**: These are the process maps created in the workshop along with various overlays – see the following points:
    - **Variations**: *Pay extremely close attention* throughout the workshop for any process variations identified. For each variation, identify which of the following applies:
      - The variation is *consistent* across all business lines or geographies. Note down these variations as **type-1 variations**.
      - The variation is *inconsistent* across business lines and geographies. Note down these variations as **type-2 variations**.
  - **Systems**: The list of systems the actors work in throughout the process.
  - **Variations**: Pay the same attention as for process variations to any *system* variations identified. Follow the same notation for variations identified.
- **Data:** What key information/attributes are used by the actors in the process. Keep this at a business-level description at this point, not an actual API name. Critically, get an understanding of data volume. This simply needs to be at *orders of magnitude* (10, 100, 1,000), not specifics.
- **Issues and suggestions:** List of pain points and new ideas for improving the process to better serve customers.

## Important note!

Create and maintain a document of the *process issues* and *suggestions* for improvements. You will need this document several times over the course of the entire Salesforce project.

Let’s have a look at what process flow chart **PME’s Salesforce taskforce** managed to create together with their workshop participants and what insights about the process were uncovered:

![](img/307881206539c45e128c538a4b6912f8_52_0.png)

Figure 2.4 – Swim lanes example for PME’s current lead management process

The lead management workshop was a remarkable success. The SMEs interacted well and eagerly shared how they work (their processes), in which system they performed the work, and what information (data) they relied on and created.

Kalinda, the business owner in PME’s Salesforce taskforce, noted down the variations mentioned in the workshop. Here’s what she captured:

- *Process variations:*
  - Some countries have a business rule where specific information has to be captured by marketing before they can input it into PME’s CRM system and assign the prospect to sales. However, this is not supported by the CRM system, but sales do complain if the information isn’t there. Other countries don’t have any business rules for what information there has to be about the prospects.
  - In some countries, it is the responsibility of the marketing department to contact and qualify prospects and update information in PME’s CRM system. In most countries, it is the sales team’s responsibility.
- *System variations:*
  - Many different email marketing systems are used by the local PME country organizations – even systems not approved by PME’s IT team.
  - Some countries use the CRM system provided by PME; others use it to a lesser extent while concurrently maintaining other data outside the CRM in a spreadsheet.

The workshop participants gladly shared issues and suggestions for improvements and innovative ideas. Kalinda noted down all these points carefully:

- Issues raised by workshop participants:
  - It is a cumbersome, manual process for the marketing coordinator to transfer leads – errors do happen, and that means wrong customer information is loaded, or worse yet, prospects do not get loaded into the CRM system, and the potential sales are lost.
  - PME’s current CRM system does not have any duplication checks, so many duplicate prospect records exist in the current system, making it hard to get an overview, and the sales team’s trust in the system is generally low.
  - Many prospect records are incredibly old and not qualified by sales. In the workshop, sales explained they were not all aware (not trained) on how to convert prospects to customers and contact persons; instead – when qualified – the sales rep simply creates a customer and contact person directly without converting the prospect.
  - Sales raised the issue that it often happens that a prospect is assigned by the sales manager to the wrong sales rep – a sales rep without the specific product insights or in the wrong geography. They then have to reassign them back to the sales manager, who reassigns them to another sales rep.
  - Sales complained about the quality of prospects assigned to them.
- Suggestions from workshop participants:
  - Marketing would like it to be possible for the current email marketing system to be able to transfer marketing qualified prospects to PME’s CRM system automatically
  - Sales would like a few key bits of information to be present on the prospect before it is assigned to them in order to effectively be able to qualify the prospect
  - Sales would like it to be a system rule that all prospects must have at least a company name, name, email address or phone number, and product interest filled out before it is assigned to them
  - *Sales would like to have all old prospects deleted*
  - *Sales suggests training marketing on how sales is organized, for them to be able to correctly assign prospects to them*
  - *Sales would like to have better training on how to convert prospects*

## Tip!

Have regular sessions with your executive sponsor to report on the progress and initial findings from the workshops. Share process and system variations identified and ask the executive sponsor whether it is the ambition and intention of the Salesforce project to also carry out **process harmonization** among your company’s business lines and local organizations.

Having analyzed and gained a better understanding of your company’s current business capabilities, it’s time for the next step in determining the capabilities of your Salesforce solution.

## Imagining new business capabilities supported by your Salesforce solution

Throughout the activities you have carried out in the Salesforce taskforce up to this point, you have gathered valuable suggestions from the SME end users for improvements to be more efficient and to better serve your customers and ease the buying experience of your customers.

Bear in mind, while end users are one of the most valuable sources of input on what to improve, end users’ **horizon of possibilities** will likely span only the edges of the existing systems and processes.

As members of the Salesforce taskforce, it is *your responsibility and prerogative* to look beyond minor incremental process improvements and reimagine what capabilities your company’s future Salesforce solution should support in order to innovate the ways your company improves its competitive advantages.

Set up a workshop with *key* stakeholders – including the senior leadership of your company – to understand the capabilities they would like to be part of the future solution.

Prior to the workshop, prepare the following two key items and share them with the workshop participants:

- Create an **executive summary** of the workshops you have carried out – highlight the issues and suggestions shared by the SMEs in the workshops
- Research what **trends** are happening in and beyond your industry and try to understand how your competitors are innovating how they market to and serve customers

The goal of the workshop should be to provide you with a list of desired new capabilities for your Salesforce solution.

*PME’s Salesforce taskforce has identified a number of new desired capabilities and enhancements to existing business capabilities, which are shown in the following table:*

| Domain | New business capability | Description |
|---|---|---|
| Marketing | Account-based marketing | Sales and marketing would like to be able to target specific accounts with account-specific content either in connection with a lead generation campaign or while a sales opportunity is in progress. |
|  | Customer | Marketing would like to create different |
