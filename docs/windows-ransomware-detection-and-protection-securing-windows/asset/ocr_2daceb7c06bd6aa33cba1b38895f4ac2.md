You can read more about the attack and its countermeasures at `https://msandbu.org/phishing-attacks-in-microsoft-teams-and-external-federation/`.

+   - As SharePoint admin, you can also define synchronization policies to define what kind of file attachments users are allowed (or not allowed) to synchronize up. Blocking files with a file extension such as `.exe`, `.hta`, `.js`, or `.iso` is a good start. You can read more about how to set up this feature at `https://docs.microsoft.com/en-us/onedrive/block-file-types`.

+   - It should be noted that features such as safe links and safe attachments provide security mechanisms for email, Teams, and SharePoint. They can still be bypassed, using legitimate URLs such as other OneDrive URLs from Microsoft or other Azure-based services, as detailed here: `https://thalpius.com/2022/03/28/microsoft-defender-for-office-365-safe-links-bypass/`.

So, it is important to note that while these security features mentioned will heighten your security, they are not bulletproof.

# Summary

In this chapter, we took a closer look at some of the countermeasures for endpoints using Endpoint Manager with features such as ASR rules and management of Defender and antimalware rules. Additionally, we learned how to ensure that we have an up-to-date endpoint and that our browser is updated on a regular basis. Then, we went into how we can protect our services using MFA services such as Conditional Access and the NPS with Azure MFA service. Lastly, we went into how we can protect our email using services such as SPF, DKIM, DMARC, and other mechanisms in Office 365 such as Safe Attachments.

As part of an overall security strategy for protecting against ransomware, implementing security mechanisms to protect end users and devices will minimize the risk of ransomware. However, we still need to have a holistic view of our environment. Therefore, it is also important that we implement security mechanisms for our virtual infrastructure, too. In the next chapter, we will look more at how to implement countermeasures for workloads running in Microsoft Azure.