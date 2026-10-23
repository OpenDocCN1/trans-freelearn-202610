If you have Microsoft 365 E5 licenses and Microsoft Defender for Endpoint enabled for your endpoints, you can also view any discovered vulnerabilities from the Microsoft 365 Defender portal. By going to `security.microsoft.com` and logging in to your Azure AD Administrator account, which has access to the service, and then going into **Vulnerability management and weaknesses**, it will list all software and vulnerabilities for your environment.

While update rings and Autopatch from Microsoft can manage Windows-native applications and products, they do not handle updates related to third-party applications that you might have within your environment. To add this feature, you need to have a third-party product such as PatchMyPC, which supports integration with Intune to handle patch management for those applications.

# Securing Microsoft Office apps

Microsoft Office applications are an important aspect of any business, and many enterprises receive daily phishing attempts or malicious content as attachments in an email.

As mentioned earlier, when I discussed some of the different ransomware attacks, many of them started as an email with a document, and when the user ran the code in the document, the attacker gained a foothold. One example is a ransomware variant, dubbed Locky, that used phishing emails containing word documents with malicious content. These documents were embedded with macros, so once the user clicked on the enabled content, the malware executable would then download an additional payload from a web server and begin looking for attached drives and network shares to encrypt.

So, having some additional security mechanisms in place for our Office applications is also an important aspect of reducing the risk of ransomware attacks.

When it comes to managing Microsoft Office applications, they can usually be done using the Group Policy settings, which can be found at https://www.microsoft.com/en-us/download/details.aspx?id=49030.

Group Policy settings are the most common way to manage configuration in any AD-based deployment. We can also use the new cloud-based configuration services that are part of the Office 365 Admin Center, which have the advantage that they can be used to manage Office 365 regardless of whether the device is fully managed or not. This means that if the user is logged in with their Azure AD account, they will automatically apply the policies to the applications.

Another good thing with this service is that when Microsoft creates additional policies, they will automatically become available within the service, so you are not required to download an ADMX to ensure the new policies are applied.

It should be noted that only user-based policies are available. If you want to apply computer-based settings, you can use the ADMX deployment template via Intune.