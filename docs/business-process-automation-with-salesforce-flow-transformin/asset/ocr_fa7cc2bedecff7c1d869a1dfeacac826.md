# Sharing data using Apex managed sharing

Apex managed sharing provides us with the ability to support specific sharing requirements programmatically through Apex or the SOAP API.

We can do this programmatically with the help of technical team members who are conversant with Apex programming, or we can use flows. Flows using Record-Triggered Flow have the ability to access and make changes to Share objects. So, you now have the power to grant or revoke specialized access to users with a click using flows.

> **Note**
> Flows using Apex sharing have nothing to do with Apex code. Before flows, we can share records in one of these four ways (OWD, role hierarchy, sharing rules, or manual sharing) with clicks, and the other option is to share records programmatically by using Apex coding. When creating Apex managed sharing, we need to create Apex sharing reasons for individual custom objects to indicate how and why sharing was implemented for the record. Flows use the same feature used by Apex code, and we are repurposing the same Apex sharing reason for flows along with Apex.

Let’s see a simple business scenario.

# Exploring a real-world business scenario

The business requirement is to create functionality to capture customers’ sensitive data for verification. Due to the nature of highly sensitive data, we would like to restrict access to contact record owners and account record owners.

Following are the business requirements in detail:

+   - Your business requested that they would like to have an object to track contacts’ sensitive information such as their tax ID, date of birth, passcode, and signature verification image.
+   - Only the contact owner and account owner will be able to access these records.
+   - If the ownership on the contact record is changed, then we need to remove access to prior owners and enable access to new owners.
+   - The contact owner will get read/write access, whereas the account gets read-only access. For this scenario, we will assume contact and account owners are different.
+   - Now, with flows, you will be able to enable this dynamic sharing feature without the need for coding.