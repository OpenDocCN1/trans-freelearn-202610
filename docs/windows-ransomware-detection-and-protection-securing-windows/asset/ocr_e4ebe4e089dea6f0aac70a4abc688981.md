> **Note**
> 
> There are more examples of the use of Kusto for detecting compromises in the official GitHub repository of this book, which can be found here: `https://github.com/PacktPublishing/Windows-Ransomware-Detection-and-Protection`.

The first attack scenario could involve a phishing attack, while the second scenario could involve a brute-force attack against a misconfigured service.

In the third scenario, services that are publicly available may be used for legitimate purposes such as VPNs, remote work, and web applications. However, it is still possible that a zero-day vulnerability or an existing vulnerability has been exploited to gain access to your infrastructure. It is important to regularly review and assess these services to ensure that they are secure.

It is also important to have a clear understanding of the external services that are publicly available, such as those used for websites. For example, a mapping of the services and frameworks in use might include the following:

+   - A VPN (Fortinet)
+   - VDI (VMware Horizon)
+   - Web applications (IIS and Apache), mostly using .NET applications
+   - Web applications running an old version of Java for an ERP system
+   - Email (Microsoft Exchange)

While it might be difficult to determine whether an exploit was used, the first thing is to determine what kind of software version each of the services runs and ensure that they have been updated to the latest version. If not, make sure that they are running the latest version, and if you suspect that there has been an initial compromise via one of these external services, try and find more information about the vulnerability and ways to mitigate it.

For instance, CIS keeps an up-to-date list of known vulnerabilities that are exploited at `https://www.cisa.gov/known-exploited-vulnerabilities-catalog`, which can also be used as a source once you have mapped out your services.

# Summary

In this chapter, we learned more about how to handle an eventual attack, how to divide the attack into different phases, how we should proceed in terms of notifying people within the organization and the authorities, and some of the ways to look for technical evidence of compromise.

It is important to note that the techniques mentioned in this chapter are just a few examples of the methods that are commonly used. The key to effective IT security is having the right tools to provide insights when needed, as well as staying up to date on the latest vulnerabilities and attack methods. The threat landscape is constantly evolving, so it is crucial to keep track of current threats. This will be discussed further in the next chapter.