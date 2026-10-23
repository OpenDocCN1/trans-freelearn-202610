AD and the surrounding ecosystem. One additional benefit this would bring is that it would effectively stop most ransomware malware loaders from triggering on regular endpoints.

Many of the different tools and scripts that ransomware operators are using today rely on access to Active Directory in one way or another to be able to map the network and try to gain access. If the endpoints are joined to Azure AD, this means that most of the tooling they have will not work.

> **Important note**
> Microsoft has different definitions of how a device could be joined to Azure AD. These are either **Azure AD registered**, **Azure AD joined**, or **hybrid Azure AD joined**. Azure AD registered devices are commonly used for **bring your own device (BYOD)** and mobile devices. Azure AD joined are commonly corporate-owned devices and only for Windows devices. Hybrid joined is a combination of a device registered in Active Directory that is then synchronized to Azure AD.

It should be noted that having a device Azure AD joined would mean that devices do not have the same access to internal resources anymore, such as file servers and other data sources or applications that rely on Windows-based authentication. This means that if you want end users to be able to access applications, those applications will need to support Azure AD-based authentication, such as **SAML** or **OAuth**.

Regular file servers running Windows Servers and SMB do not support Azure AD-based authentication by default, so you would need to either move to another file service such as **Office 365** with **OneDrive**, or a file service that supports SMB in combination with Azure AD-based authentication.

In addition, applications that rely on Windows-based authentication no longer provide SSO because the endpoints do not rely on the use of **Kerberos** or **NTLM** anymore. This means that you would need to look into other ways of publishing your applications to end users that would be supported together with Azure AD.

One final thing that should be mentioned here is that Microsoft recently introduced a feature called hybrid cloud trust, which allows Azure AD-based and hybrid joined devices to authenticate to an on-premises file server. Due to previous iterations of this service, it has several requirements, including a PKI service to issue certificates to the end user machines to use it.

Using Azure AD Kerberos, cloud Kerberos trust eliminates the need for PKI. Authentication tickets can be issued by Azure AD for one or multiple AD domains, and Windows can utilize them for authentication with Windows Hello for Business, as well as by accessing traditional AD-based resources. The on-premises domain controllers remain in charge of Kerberos service tickets and authorization.

# Vulnerability and patch management

Within any environment, an important aspect is vulnerability management. Vulnerability management is a lot more than just looking after software vulnerabilities, but this book will only be focusing on