
Rely on your policies and procedures that have been developed to help determine the thresholds of an incident and its impact on the business. These documents should spell out what needs to be done to prevent an incident from spreading. Next, you will want to work on eradicating malicious activity from your environment. The policies and procedures should also detail what is and is not considered an incident. Not all events become incidents. However, all incidents started as events.

## `RS.MA-02`

*Incident reports are triaged and validated.*

FIPS 199 details the criteria for categorizing events and applies severity levels to them. These are ranked as low, moderate, and high, and are stated as follows:

- **Low**: There is a limited adverse effect on the organization due to a loss of confidentiality, integrity, or availability
- **Moderate**: There is a serious adverse effect on the organization due to a loss of confidentiality, integrity, or availability
- **High**: There is a serious or catastrophic adverse effect on the organization due to a loss of confidentiality, integrity, or availability

These criteria will come in handy when determining how to respond to an incident. If an incident is low, you will know how quickly you should respond (if at all), whereas you will know that a high incident requires you to respond immediately.

You will also want to have reports generated and reviewed to determine which events became incidents. These reports should also spell out what should not have been reported as an incident. This will save you and your team valuable time not only when you respond to an incident but also when a reported incident turns out not to be one, so you can fail fast.

Once an incident has been validated, work to contain the incident as soon as possible. When containment has been completed, at least to the best of your ability, begin work to eradicate the adverse event.

A great example of incident report writing can be found here `[*1*]`.

## `RS.MA-03`

*Incidents are categorized and prioritized.*

While we categorized our adverse events in `RS.MA-02`, low, moderate, and high categories are not enough. Each adverse event should be placed into its own bucket or types of attacks. For example, we could categorize them as one of the following:

- Business email compromise
- Account takeover
- Ransomware
- Breach

As we have developed SLAs, created impact thresholds, and learned about the scope of the incident, we can then prioritize how we respond to it. You may target your Active Directory servers as they are of the highest importance within your IT resources. From there, you may decide to prioritize your databases or SCCM environments depending on the type of attack it is. This information should be contained within your CMDB or a similar tool.

When responding to an incident, it may also be necessary to take a step back and observe what they are doing. By doing this, you can gather a lot of intelligence on how an adversary conducts an attack. This information can then be turned into playbooks for future attack remediation. Do not do this for long, however, as eventually the attackers will get what they want.

## `RS.MA-04`

*Incidents are escalated or elevated as needed.*

With this control, you must monitor the status of the incident or incidents that you may be experiencing at any given time. If an incident is out of control and you need assistance, you need to escalate the issues being faced to others either inside or outside the organization.

If you are escalating internally, you may need to call in additional employees to assist with troubleshooting a given issue or help with containing an outbreak. This could include those on the networking or server side of IT whose expertise will be needed. If you are escalating externally, you will need help from trusted third parties such as a managed security provider, ISAC, or law enforcement. For example, you may have your firewalls managed by the MSP and will need to get a hold of them quickly to block a threat coming into your network. Having contacts at these facilities is beneficial when needing assistance.

Leverage your after-action reviews to better understand whether the process is working as intended. An after-action review is meant to highlight deficiencies in your response plan. Take into account how the response occurred and whether it was appropriate for the situation.

## `RS.MA-05`

*The criteria for initiating incident recovery are applied.*

Once the incident has been contained, and the threat has been eradicated, look for possible key areas where you can begin recovery. This is where you will need to have runbooks created for various types of incidents within your organization. These runbooks should be diversified into multiple categories, much like we did in `RS.MA-03`.

A runbook for business email compromise or account takeover will look different than the runbooks for ransomware. Each runbook should also discuss the containment and eradication phases of the incident. Having this information will assist with cleaning up the incident while trying to minimize the amount of downtime caused by the event.

Not all incidents are the same and they should not be treated as such. A business email compromise may or may not disrupt service for an employee or the entire company. However, a ransomware attack has the possibility to take down a network for extended periods of time. This type of attack will cause disruptions of service and it is up to you and your team to minimize the amount of disruption. (For more information on how to create IR playbooks, take a look at `[*2*]` in the *References* section.)

# Incident analysis (`RS.AN`)

In this control family, we will discuss the actions or steps you need to take during and after an incident has occurred. This will include recording all the steps taken during the incident, performing an after-action review, and preserving a chain of custody for the incident report being generated.

## `RS.AN-03`

*Analysis is performed to determine what has taken place during an incident and the root cause of the incident.*

In this control, we are trying to determine the steps that led up to the incident. This will require that we create a timeline of the incident itself, from when the event was determined to be an incident, to the steps that the IR team took to contain and eradicate the incident from the environment. Creating this timeline will also help us determine the root cause of the incident.

In the development of the timeline, take the decisions that lead to making the event an incident into account. There was a decision that was made to enact the IR team and begin developing the response to the incident. Record everything in the timeline as well. From decisions that were made to contain the incident to conversations that were had, everything should be recorded.

Also, record the decisions that were made in regard to the various systems and services that are in the environment. For example, if an adversary took advantage of a vulnerability, was that vulnerability discovered within the environment? Was there an exploit from the CISA KEV that pertained to your environment? If so, what was the decision that was made to either mitigate or accept the vulnerability?

This timeline is outside of the policies and procedures that were developed. However, the discussion around the creation of a timeline should be part of those documents. It is critical that you maintain this timeline of events to help paint the picture of what your IR team did and the decisions that were made.

## `RS.AN-06`

*Actions performed during an investigation are recorded, and the records integrity and provenance are preserved.*
