
In the SSP, you should also record all the cybersecurity controls implemented on the device. This will assist you in determining what controls are lacking and where you need improvement. For example, in your SSP, you will record all the security configurations for a given IT resource. If, for instance, you were lacking antimalware software on the system, then that would be a risk that should be recorded into a risk register as part of the SSP. You will then be able to determine your response to a lack of control so that you can plan for additional resources, if needed, to reduce the amount of risk associated with that asset.

## ID.RA-06

> *Risk responses are chosen, prioritized, planned, tracked, and communicated.*

In the *Govern* function, we learned that there are positive and negative risks associated with our cyber program. When responding to negative risk, you must choose how to handle the risks identified for a given IT resource. Negative risk responses include the following:

*   **Accept** – You accept the risk and decide to do nothing to mitigate it
*   **Mitigate** – You resolve the risk
*   **Transfer** – This involves migrating the risk to a third party, which is typically a cyber insurance provider
*   **Avoid** – This is typically where you would scrape the project due to a high amount of risk that is too costly to mitigate or due to other business requirements

When accepting the risk, you plan on doing nothing with the risk identified. This means that you accept whatever risks are associated with that IT resource. Mitigating risks is when you reduce the risk to acceptable levels, including implementing a lacking control or reconfiguring a control to ensure that it meets or exceeds risk reduction. Transference of risk is the event that allows you to move the risk to a third-party vendor. Most of the time, this includes using a cyber insurance provider or transferring the hosting of a service to a SaaS provider or CSP. Lastly, avoiding the risk means you intend not to implement the product or resource in the environment regardless of its placement. The risk is just too high.

Whatever you intend, you should also have a project plan or **plan of action and milestone** (**POA&M**) to perform a particular risk response. The POA&M is meant to help you and the organization handle risk and your intended response. You can place these risks into a risk register that can be discussed with stakeholders to ensure the risk is handled appropriately.

## ID.RA-07

> *Changes and exceptions are managed, assessed for risk impact, recorded, and tracked.*

This control objective will require that you create a new change management process if one does not exist. Change management involves identifying required changes in the environment, determining how to roll back the changes if they are unsuccessful, and communicating those changes to stakeholders.

You need to document the changes being made to a given system. This documentation can be in the form of a change log or another type of format to allow you to track the changes being made to a given system. This change log can also be useful for others to understand how an IT resource was configured and the changes that were made.

The change request should include the risks of making or not making the change, the requirements for making the change, its process, rollbacks, and who is responsible for the change. The change request should also include an approval process to allow others to review the requested change and ensure that the change being made is appropriate.

## ID.RA-08

> *Process for receiving, analyzing, and responding to vulnerability disclosures are established.*

We have discussed ISACs and their importance. Often, an ISAC is only as good as the information it receives. The ISAC must not only send critical risk information to its members but the membership must also share sensitive cyber risk information with the ISAC.

Ensure that you provide critical cyber risk information to your stakeholders, managed service providers, ISACs, or other institutions you conduct business with. This will assist in ensuring that everyone has the most valuable and up-to-date cyber risk information needed to make good decisions on how to handle risk.

If you provide third-party services to your customers, provide them with information about any issues you and your organization may be experiencing to be transparent with those you do business with. This will improve you and your organization and strengthen the relationship between you and your customers.

## ID.RA-09

> *The authenticity and integrity of hardware and software are assessed prior to acquisition and use.*

There are a few different ways you can view the authenticity and integrity of the software and hardware you use. Software developers use cryptographic signatures to ensure that the software being installed on a device is legitimate. Without this signature, any software will not load, preventing someone from installing malicious software onto the device.

Digital signatures are one way of doing this; another way is to run a hashing function against the software. When running a hashing function, you get a unique character value used to determine the integrity of the software. If the hash value were different, that would mean the software has changed and is not the valid software developed by the manufacturer. We can also do this with regular text files. For example, a file that contains `This is a hashed file` would result in a hash value as follows:

`2c0e7d464573504149e86e8be26a1118b077c7dbee562086900df1943061bae7`

If we were to change the contents of the file to `This is a hashed file!`, the new results would be as follows:

`10b481d359a851de7efb58b1cc4f0b237014e37be60936ae22da52ac10eb7388`

The contents of the hash values are completely different. The hashing function can be run against files, compiled software, multimedia, or anything on a system.

Be extremely cautious of software developers that do not provide one of the two previous functions. Without a mechanism for checking the validity of software, it could lead to supply chain issues, where the software was manipulated without the developer’s knowledge. This could also mean that malware was inserted into the application without anyone’s knowledge.

## ID.RA-10

> *Critical suppliers are assessed prior to acquisition.*

As we did with evaluating risk and applying a criticality tier to our IT resources, we should apply the same scrutiny to our suppliers. Depending on the criticality of the vendor’s service, this could lead you to want to conduct a risk assessment against the vendor as well.

Third-party vendors are an extension of your network and workforce. They hold onto your sensitive data while providing much-needed services. Because of this, you should be performing evaluations against these suppliers to ensure that their security model reflects your or your business’s expectations when safeguarding data. We previously discussed using SIG Lite or COBIT in [*Chapter 3*](#B21915_03.xhtml#_idTextAnchor046) to perform these evaluations. This is a much-needed step in your evaluation of their security controls.

# Improvement (ID.IM)

We have put considerable effort into identifying IT resources and performing risk assessments around them. This effort must be placed into a program where continued efforts and improvements are made to ensure that we do not lose sight of identifying and classifying our resources. This next family of controls is meant to ensure that continual improvement occurs to streamline our processes better.

## ID.IM-01

> *Improvements are identified from evaluations.*
