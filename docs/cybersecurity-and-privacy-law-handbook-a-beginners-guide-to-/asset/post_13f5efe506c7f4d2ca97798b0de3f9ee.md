
## 7.5.3 – Management of recorded data

The standard says that recorded information required by the ISMS, as well as the standard itself, must be accessible, usable, and sufficiently safeguarded against destruction or loss of integrity and identity, regardless of its internal or external origin. For the correct management of recorded information, the organization must provide distribution, retention, access, use, retrieval, preservation, and storage methods, as well as control and disposal procedures.

Documented information required by the information security management system and by ISO 27001 documentation must be appropriately checked to ensure (note: the originating clauses are in brackets):

- ISMS scope (documents) (4.3)
- High-level information security policy (documents) (5.2)
- Risk assessment methodology (documents) (6.1.2)
- Risk assessment report and risk treatments (record) (6.1.2, 6.1.3, 8.2, 8.3)
- Statement of applicability (documents) (6.1.3 d)
- Information security objectives (documents) (6.2)
- Evidence of competencies (record) (7.2)
- Documented information as required by the ISMS (documents and record) (7.5.1 b)
- Documents and records required by ISO 27001 (documents and record) (7.5.1 a)
- Monitoring and measurement results (record) (9.1)
- Internal audit program and results (record) (9.2)
- Results of management review (record) (9.3)
- Non-conformances and results and corrective action (record) (10.1)

> Tip
>
> The terms in bold are the respective clauses of Annex A of ISO 27001. Concerning non-conformance, ISO defines them as “the failure to meet one or more requirements that are outlined throughout the mandatory clauses.”

The distinction between a document and a record is that records are time-stamped proof of activity. Periodically, documents are examined and updated. Typically, they are versioned. In addition to the records mentioned previously, other relevant documents may include the following:

- Evidence of the risk owner’s consent for specified controls or risk acceptance
- Visitor records, including CCTV photos and access logs
- Security incident logs and root cause analysis
- Rectifications and enhancements

> **Tip**
> I think you imagine a truckload of documentation to be prepared and ready, but a high percentage of the companies I personally assessed needed "only" the paperwork required to fulfill the criteria of clauses 4 to 10 consisting of around 25 to 35 pages in total (excluding title pages and version control, I mean the version number). Among them are the outcomes of the management review, the internal audit report, the risk assessment, and the performance evaluations. This amount of pages for clauses 4 through 10 is generally intended to be independent of the organization's size; nonetheless, bigger organizations have additional paperwork related to, for example, performance management and internal auditing.

The second major sort of documentation is information security policies, which are more diverse. 20 to 40 pages of documentation have been required for the ISO 27001 implementations I've performed for smaller organizations (with maybe a few thousand users). Larger organizations need more policy-related paperwork, although it typically did not exceed 60 pages.

The third form of documentation is the documentation/records generated as a consequence of running the controls/procedures, such as visitor records, change control records, and penetration test reports, where the type of documentation generated is highly reliant on what is implemented.

If you have more than this, you may have made things more complicated than necessary. Or maybe you do need it all to effectively handle information security concerns. But I doubt it.

So, once everything is in place (policies, controls, procedures, and so on), you can have your audit and, if it's successful, you can obtain your ISO 27001 certification. But can you later lose it?

Yes. Auditors will periodically return on-site (or remotely) and request evidence that you are continuing to comply with all regulations. If you cannot provide evidence that you are, they will revoke your ISO 27001 certification. So, we must ensure that we continue to comply with all ISO 27001 requirements.

How can you ensure you do so?

- Be familiar with the primary tenets of your information security policy.
- Be familiar with the primary concepts of the content of the intranet's primary information security pages and complete the yearly training on information security.
- Know where to locate the organization's rules and procedures, especially those that pertain to information security and apply to you.
- You are not required to grasp every detail, but you should understand the fundamentals.
- Read, understand, and agree to the terms of the acceptable use policy annually.
- Understand your role in helping to secure the organization's information and the significance of that information. For instance, avoid revealing your password and handle sensitive information with extreme caution.

Be aware that if the organization has a policy or procedure that specifies how things should be done, it is crucial that the policy or process is accurate. For instance, if a policy or procedure states *All visitors must be accompanied by a relevant member of staff*, it is imperative that all visitors without exception. If a policy or procedure is not consistently adhered to in practice, either the policy or procedure must be revised, or individuals must ensure that they adhere to it.

It is vital to record instances of disparity between an organization's policies/processes and what is really in place in order to determine whether the policy/process should be amended. Remember that a policy or practice does not need to be recorded in order to be legitimate and adhered to.

Be cautious when adopting *improvements, shortcuts, or changes* that do not align with the organization's rules or processes. You should get formal approval for this action and, if required, modify the policies/processes properly.

You should also report anything that seems to have the potential to expose sensitive information to unauthorized parties, for instance, if you notice that a door lock is broken or that critical papers have been left on a desk overnight.

Remember the fundamentals of security, such as the following:

- Use extreme caution when links from people external to your organization emails
- Clear your desk papers with confidential information while you are not present
- Lock your workstation's display before leaving your desk
- Deposit all rubbish containing confidential information in the designated containers

In previous paragraphs, I translated the main requirements of Clause 7 to demonstrate how to use them more easily. Of course, as we have said many times, ISO 27001 is continuously evolving and improving. You can improve on the basic requirements proposed within the clause. For instance, you can require internal training every 3 months for all your employees and install document shredding machines to destroy documents. It's entirely up to you; there is always room for improvement. Just remember: don't excessively implement bureaucratic or unsustainable procedures; they can be a waste of time for your employees and your company.

## Summary

Well, that’s another chapter filled with a lot of information that we’ve completed. You’ve learned what the requirements, steps, and documentation to provide to fulfill ISO 27001 accomplishment are. At the same time, you should be aware that you can lose the status of ISO 27001 certified company just by non-compliance with the relevant information.

In the next chapter, we’ll deep dive into risk management, policies, and controls.

## Risk Management, Controls, and Policies

One of the most important things to sort out, aside from preparing all the documentation needed and confirming support from management, is to have a list of all the entity's assets. In this chapter, we will be covering risk management, data classification, and the controls defined within ISO 27001.

An asset is a resource having economic worth that a person, business, or nation owns or manages with the idea that it will produce future benefits. The balance sheet of a firm lists its assets. They are acquired or established to raise the value of a company or to boost its operations. In our context, an asset is defined as any goods or services, tangible or intangible, that are considered part of an entity.

So a firewall, for instance, is an asset; a pen is an asset; but also the documents on the local or online repository of the company are considered to be an asset, as well as that server that they were supposed to dismiss five years ago, but for some reasons as unknown as the Bermuda triangle, is still there.

Why in the world is a document (MS Word, Excel, PowerPoint, you name it) considered an asset? Well, of course it's not due to the document(s) themselves, but because of what those documents are worth: let's suppose that you are the King of Reign1 and your plans for invading Reign2 and therefore becoming Emperor of the Galaxy are in your safe waiting to be used – how much are they worth? Or let's suppose that you've got 50 kilos of bearer shares? What is their value? Or all the notebooks that you used while at primary school, how much are they worth, according to your parents?

You got the point: if the shape of water depends on the container that encloses it, the value of a document does not depend on the physical document but on the information it contains.

In this chapter, we will cover the following topics:

- Elements of project risk management
- Data classification
- ISO 27001 controls
