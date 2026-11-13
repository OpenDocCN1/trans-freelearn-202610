
We all experience some type of risk each and every day. You woke up this morning, good job! Made breakfast, wait – was the milk spoiled? Drank your coffee – hope it was not hot enough for you to burn yourself. Then you grabbed the keys and jumped in the car and made it just before 8 AM to clock in on time. How did you get there? Did you speed? Did someone cut you off? Instead of taking the car, I guess you could have ridden the bus or taken the train. Wait, you still could have gotten into an accident.

You see, you took a risk by even getting out of bed this morning. You could have drunk spoiled milk, sipped scorching hot coffee, or sped to get to work on time. These risks are all second nature and you probably don’t even think about them. They seem common; it’s almost like you’re numb to the fact that almost everything can be dangerous.

We can sometimes take the same stance when it comes to cyber risk. There are so many dangers when it comes to IT-based systems, you may feel like you want to encase your systems in concrete and drop them in the middle of the ocean, also no network cables! Taking this stance, however, could spell the need for you to find a new job because no one would be able to do their work.

Cyber risk can sometimes feel subjective, and it is. One company’s view of cyber risk is not the same as the next. They could be two separate companies, in the same town, performing the same function, with the same market cap, and still have two differing views of cyber risk. Decentralized IT teams make this even worse by having completely different views of risk while working for the same company. Why is that? Experience? How much money is the company willing to spend on risk reduction? Will the organization supply you with enough personnel to perform all that is necessary?

Let us start with how we evaluate risk. Would you spend $100 to protect $50? No. On the flip side, would you spend $50 to protect $100? Possibly. There are several factors that should be considered for the asset, such as its age or its ability to be upgraded. While these are just examples, they tell us how much we may value an asset and the information that resides on it. Just because the value of the asset may be $100 does not mean the value of the information on the asset is not worth way more than that. Now that we know that how we deal with risk is completely different, how about we take a closer look?

There are four primary ways that we deal with risk, all of which are meant to perform a task when risk is identified:

*   **Mitigate**: This is where we do something about the risk. It quite possibly could be as simple as applying a software patch or buying new firewalls for the network. It could also be retiring a legacy system.
*   **Transfer**: We are transferring the risk to a different party. We mostly think of risk transfer as purchasing a cyber insurance policy. Transferring could also mean that we transfer the security of our network to a third party such as AWS or Microsoft Azure.
*   **Avoid**: Avoiding risk involves scrapping the project because the risk is just too high. You have a legacy system in your environment; instead of mitigating the risk, you decide to move the legacy system out of production. This could also mean that you do not take on a project due to its high risk.
*   **Accept**: This is typically the last way to deal with risk because it could open you up to significant issues. With acceptance, you do not plan to put any mitigations in place, you do not plan to transfer the risk to an insurance company, and you cannot avoid it. In this case, you ultimately accept the risk. This can lead to a compromised system, so move forward with caution.

These are the four main ways to deal with risk, but that does not tell us about where risk begins or who is responsible for it. In this next section, we will discuss the three tiers of cyber risk.

# NIST RMF

The NIST RMF takes a simplistic approach to evaluating cyber risk throughout the organization. At the top of the triangle, we start off with tier 1, which is primarily made up of the BoD and the **Executive Leadership Teams (ELTs)**. They are responsible for defining the risk and its thresholds. When it comes to tier 2, we take the definitions and place them into policies, standards, and procedures. These are then pushed by the enterprise architecture team to define the standards to be used by the organization. Lastly, tier 3 is the tier where the IT systems reside:

![Figure 9.1 – NIST SP 800-37 RMF](`image/B21915_09_1.jpg`)

Figure 9.1 – NIST SP 800-37 RMF

## Tier 1

Organizational risk is first defined by the BoD and ELTs. They are the ones that state what their level of risk or their risk tolerance is. Back when I said cyber risk can be subjective, this is what I meant. There is no standard definition that states that you must patch a vulnerability within one day, nor is there a regulation that states that you must be protected by cyber insurance as is the case for car insurance.

It is subjective in that they tell the company what should and should not be done based on experience and what they feel is in the best interests of the company. It is up to you, as the cyber professional, to ensure that the risk of doing (or even not doing) a particular task will cost the company. The company could state that a secure email gateway is just not in the budgets for this year. While you have expressed your concerns about not having one, it is ultimately their decision. Now it is up to you to still ensure that email is protected with the tools that you have.

## Tier 2

Throughout the chapters that covered the NIST CSF functions, we discussed documentation. Tier 2 is where that documentation is created and enforced. Tier 1 is where the organizational risk is defined. Now we can take those definitions and put them down on paper. In the next chapter, we will discuss how to create a **Policy, Standard, and Procedure (PSP)** framework, but in this chapter, we will briefly discuss what that means.

A policy is a statement document that shows intent. These documents are meant to be high-level in that you can freely hand them out to your friends, family, and loved ones and not feel like you are giving away the keys to the kingdom. For example, a policy document could state that all hard drives shall be encrypted; great! However, the policy document should not state how that encryption is being performed or what the encryption algorithm being used is, nor should it state where the keys should be stored. That is where standards and procedures come in.

The **Enterprise Architecture (EA)** team is responsible for establishing standards. EA takes the requirements from tier 1 and implements those in the environment. In addition to implementing standards, they are responsible for writing up standards documents. For example, the BoD and ELT state that there needs to be additional focus on preventing attacks from the internet. The current set of firewalls only block `source/\\destination/port/and protocol`, which was fine in its day, but the organization needs better protection. To prevent new attacks from coming into the network, EA decides that there need to be IDS/IPS capabilities.

The organization has already deployed a set of firewalls and standardized on a given manufacturer. Now, implementing the IDS/IPS configuration will require an evaluation of the current set and whether the functionality can be utilized. If it cannot, then you will need to begin looking at different hardware. While you may have standardized on Cisco in the past, the new firewalls may be from Cisco again, or a different brand. The point is that once a set of firewalls has been chosen, the EA’s responsibility is to ensure that this new brand is used throughout the organization.
