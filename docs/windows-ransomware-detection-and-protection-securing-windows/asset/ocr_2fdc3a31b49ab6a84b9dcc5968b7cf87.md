When a user tries to connect to a service, the ZTNA product can, depending on the configuration, check the following:

+   - Is the device safe? Has it been updated? Does it have the necessary security configuration in place?
+   - Is the user coming from a known location? Has the user authenticated using MFA?
+   - Is this normal user behavior or is it abnormal?
+   - Does the user have defined access to that service?

So, let us look at some examples of how access can be differentiated for users to an internal service:

+   - User 1 tries to access an internal web application, coming from a known location
+   - User 1 device has a security baseline in place, which includes anti-malware being installed and Endpoint Detection & Response (EDR) agent being enabled
+   - User 1 is granted access given that they complete an MFA prompt
+   - The ZTNA service constantly evaluates whether the state changes, such as the user location or device risk

Let’s look at another user who is trying to log in from an unknown device or a device that does not have the necessary security configuration in place:

+   - User 2 tries to access an internal web application from a known location
+   - User 2 device does not have any security configuration in place, and anti-malware and EDR are disabled on the device
+   - User 2 is not granted access to the service based on the combination of their identity and device postures and access policies

Now while zero-trust and the different pillars are a set of security design principles, it would be impossible to provide ZTNA without a certain set of technologies. As I mentioned, there are a lot of vendors on the market that provide so-called ZTNA services to ensure that you can verify a user and their device, attributes, and context before they can access a service or data.

This is also a change in the way that we provide access since a user by default should not have explicit access to internal services (such as with an AD-based computer, which essentially has explicit access to internal resources once it has been authenticated). Hence, if you want to implement a zero-trust-based security architecture, you should look into moving your managed endpoints to Azure AD.

While many good vendors provide many of the same features, I will give an overview of three different vendors within this market, what kind of features they provide, and how these features can help reduce the risk of ransomware.