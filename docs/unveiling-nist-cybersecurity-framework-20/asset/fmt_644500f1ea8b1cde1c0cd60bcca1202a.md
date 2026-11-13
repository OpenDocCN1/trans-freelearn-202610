
# Access Control (PR.AA)

## SAML 2.0 and Identity Providers

Authentications should be protected from session hijacking, password spraying, and MFA push fatigue and ensure encrypted communication. These are just a few safeguards to consider when choosing a single sign-on provider. There are a few standards that have been developed to help with this, and one is **Security Assertion Markup Language (SAML) 2.0**. SAML 2.0 provides authentication through the **identity provider (IdP)**. The IdP is used to authenticate the user once and then provide a token assertion to the SP.

The communication between the IdP and the SP is protected against attacks, and you can enforce MFA. Another benefit of using a SAML provider is that you can request the user to re-authenticate, making it possible to kill all active sessions in the event of an account takeover.

## PR.AA-05

*Access permissions, entitlements, and authorizations are defined in a policy, managed, enforced, and reviewed, and incorporate principles of least privilege and separation of duties.*

As we grant credentials to our employees, we provide them with access to a given resource. Over time, those credentials can gain what is called **permissions creep**, where an employee who has been transferred to a different department or promoted maintains access to systems they no longer need. Often, when this occurs, the credentials the employee uses maintain their old permissions while gaining new ones. We must look at restricting credentials to only those permissions that employees need to do their daily jobs.

We can also look at **attribute-based authentication control (ABAC)**, which considers much more than just a username and password or the use of MFA. ABAC can consider the location of the user or credential being used, the time of day, what they are trying to access, and their intentions for the access, whether it’s to read or write to the document.

We should set aside time for our employees to review access permissions. This will help reduce the number of rights a particular user has, further securing the network. User access reviews should occur at least annually or when a new request is made to change the permissions of a user’s account.

## PR.AA-06

*Physical access to assets is managed, monitored, and enforced commensurate with risk.*

Not only do we need to protect the logical side of our IT resources, but we also need to secure the physical side. Data center security should always consider new ways of protecting assets as many logical security configurations can be circumvented. Administrative accounts can be reset using a physical CD inserted into the server, network equipment can be circumvented by setting the registry to a given value, or devices can be reset by pressing a button for 10 seconds.

Many data centers will not only physically secure the environment but also implement several policies and standards for how people are escorted throughout the facility. For example, an employee may want to enter the data center to repair a failed device. We should check to see whether they are even allowed into the data center. Additional background checks are often performed to check whether the person who is trying to enter the facility is trustworthy. We can even decide whether employees can enter the facility by themselves or whether they have to be escorted by a data center official.

We must also establish a reason for an employee entering the facility. This is typically done through a work order requesting access to the facility due to an outage. Even though they may have been granted access to the facility, that does not mean they can go in and out of the data center anytime they want. There must be a reason.

Regardless of whether the employee can gain access to the facility, we still need to verify them. This means requiring the employee to present their employee ID card and sign in on a sheet of paper stating the reason for the visit, as well as presenting the work order number providing them the ability to enter the facility and their reason for visiting.

This goes for guests and vendors as well. Many guests or vendors should not be able to enter the data center without being escorted. If they do not work for your company, you may not want anyone to access the data center without being escorted.

The facility should have several safeguards deployed, including security cameras, guards, and alarm systems. This is to monitor the environment and provide traceability when something goes wrong. We can also have mantraps when entering the server room, requiring one last authorization prior to entering. We can use locked cages and only provide a key to the specific cage in highly secured environments. RFID tags can also be used on the device to trigger alarms if a server were to be removed from the facility.

It is necessary to verify that a person is who they say they are, both physically and virtually. This is to ensure that the correct individuals are performing the intended duties. Security awareness and training is also a critical step when educating your users on the appropriate steps to take when faced with a threat or identified risk. Let’s learn how.

# Awareness and Training (PR.AT)

The employees at your company are your eyes and ears when it comes to cybersecurity. They are your first line of defense. The employees will often see new threats against your facility before you do. Security awareness and training are not meant just for the cybersecurity professional; it is meant for everyone in the company.

## PR.AT-01

*Personnel are provided with awareness and training so that they possess the knowledge and skills to perform general tasks with security risks in mind.*

There is a difference between security awareness and training. **Security awareness** is meant to provide generalized cybersecurity awareness within the facility. This can mean posting flyers or posters throughout the organization promoting cybersecurity topics. This is meant primarily for those outside of IT.

**Security training**, on the other hand, is more targeted or focused on cybersecurity topics that provide in-depth knowledge of a particular area. For example, we may be aware of phishing scams or the dangers around the malicious use of QR codes. A focused security topic would be training our firewall administrators on the new firewalls that we just put in place.

Cybersecurity awareness should occur on a regular basis. The sweet spot for this is to do it at least once a month; any longer than that and studies have shown that people lose focus of what they are supposed to do when faced with a risk [*1*]. Studies show that performing a security awareness program just once a year is as effective as not doing anything at all. This is why performing these programs regularly is beneficial.

Follow up your awareness program with phishing campaigns to reinforce the necessity to use caution when interacting with emails. As phishing is on the rise, you will want to ensure that your employees are being trained on what to do when they encounter a phishing email in their inbox.

As mentioned, focused training is also necessary, but this is typically for IT-related staff. Training can occur at least once a year for your IT and cybersecurity staff. Maybe you need to send them to a class on using firewalls or provide training on a new **intrusion detection system/intrusion prevention system (IDS/IPS)** appliance that was just installed. Focused training is also more expensive than awareness training. Depending on the type of training being requested, it could cost thousands of dollars. This is acceptable for specific employees, not the entire staff.
