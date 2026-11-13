
I highly suggest keeping a risk register for each scoped environment. This way, it minimizes the number of risks that you must search through when looking at the resource. I also highly recommend building an enterprise risk register that not only encompasses the risks identified for the scoped area but also showcases all the risks identified in the organization.

Creating an enterprise risk register will bring in all of the identified risks for each scoped environment. The highest-ranked risks should be at the top of the enterprise risk register for all IT resources in the environment. This will help you identify the risks associated with everything, not just a couple of assets that you focused on.

Include risks from other departments as well. Regulatory and reputational risks are often overlooked when developing a risk register. As we will see in later chapters, reputational risk is often hard to remediate as you are trying to promote yourself as a secure organization after a breach or other mishap.

## `GV.RM-04`

> *A strategic direction that describes appropriate risk response options is established and communicated.*

Once a risk has been identified, you must do something about it. We previously discussed the need to implement policies and standards for how to handle risk. However, how do we accept or even transfer the risk if we don’t intend to perform any further mitigations to eliminate the risk?

Organizations should have a way of accepting the risk posed to the organization. There should be an approval process that allows those in the ELT to review and either accept, mitigate, transfer, or avoid the risk altogether. If risks are mitigated, great – the job is almost complete. If there are residual risks, or risks that the ELT will just accept, there must be an approval process.

This can be accomplished by either gathering signatures or using some type of stamp of approval from an ELT member stating that the risk poses little to no threat to the organization. What if a risk is unavoidable? Many organizations will also purchase cyber insurance to transfer that risk to a third party. You should have these discussions with the insurance provider – often, their risk tolerance levels are lower than your organization. Remember, the insurance company is also on the hook if/when something happens to your organization. They, like your organization, are in the business of making money, not losing it.

## `GV.RM-05`

> *Lines of communication across the organization are established for cybersecurity risks, including risks from suppliers and other third parties.*

Lines of communication should be open when discussing cyber risk in the organization. While identifying cyber risk and how we communicate that risk starts at the top, it must flow downward throughout the organization. Your enterprise architecture team, engineers, and analysts are the ones who are implementing the controls and ensuring that the risk is removed from the environment.

There must be a feedback loop from the engineers and analysts that flows back up to the top so that the ELT can understand any shortcomings. Maybe a particular remediation cannot be accomplished because you lack a certain control, or the control that you do have can’t resolve it due to a lacking feature. This must be communicated and discussed to ensure that all parties involved are working toward a common goal.

Does your plan also include how to communicate and respond to an incident that may have occurred in a different area or department in the organization? For example, you tell employees not to tailgate each other when coming into the building. However, this would require that you have an open communication policy if someone identifies that this has occurred. In that plan, you should have who to contact and when so that the employees know what to do. Maybe you have the employee contact their immediate supervisor or manager when an isolated incident occurs, and it is escalated to your CISO if this happens multiple times.

This also requires that your third-party vendors and suppliers do the same thing. Ensure that you have contact information regarding how to respond if/when that vendor has an issue. Often, when partnering with another provider, they will require that you have their contact information in your incident response call tree so that you know who to contact when something does come up.

## `GV.RM-06`

> *A standardized method for calculating, documenting, categorizing, and prioritizing cybersecurity risks is established and communicated.*

Risk is often calculated using the following formula:

`Risk = Likelihood * Impact`

These risk scores should also be included in the risk register discussed previously. This risk calculation will help identify the most important risks and prioritize them for remediation efforts.

How do you prioritize the use of these numbers and their overall risk to the organization? This is often less understood than just using the calculations alone. However, you should not only prioritize based upon the calculated risk number in addition to who may have access to the resource. For example, let’s say that you have several high-ranking risks on an internal asset that’s behind two different firewalls and only 10 people can access it. You also have a high-ranking risk and several moderate risks on an asset that’s public-facing. Which one should you remediate first?

This question is posed all the time and many people miss its significance. I once had a manager explain this to a group of junior analysts. Years ago, back before the internet, people had to drive to a bank and either go inside or use the drive-through for their needs. While there was a risk of someone robbing the bank, it was minimal as the person had to physically be at the bank to rob it.

Now, with the internet, you don’t have to physically be there – you could physically be anywhere in the world and try to get funds out of the bank. The risk profile was expanded considerably due to anyone with an internet connection being able to now be a customer, or worse, do something nefarious.

The risk here is that billions of people could expose that weakness on the public-facing website. While there may be a `CVSS 10.0` vulnerability on that internal website sitting behind two firewalls, it is less risky than say a `CVSS 7.0` vulnerability sitting on a public server where 30 billion devices can access it.

## `GV.RM-07`

> *Strategic opportunities (that is, positive risks) are characterized and are included in organization cybersecurity risk discussions.*

Many of us think of cyber risk in the negative. For instance, let’s say a new vulnerability was identified and you have two systems that are susceptible to it. This is a negative risk. However, there are positive risks too and these come in the form of risk reduction measures or opportunities to obtain countermeasures to correct identified risks.

You should perform an analysis of negative and positive risks. This will assist conversations with staff members and the ELT about how you are minimizing negative risks in the environment. Positive risk can be accomplished by reducing the risks associated with the asset, which can align with the overall business goals and objectives.

You’ve identified that you aren’t minimizing your vulnerabilities due to a lack of a vulnerability identification system. This should prompt you to acquire an application to perform these scans. The acquisition of the mitigating control is a positive. You have now identified a way to gather vulnerability information and developed a strategy for how you want to minimize those vulnerabilities in the environment. A positive would be that you remediated, say, 10 vulnerabilities in the course of a few short weeks.
