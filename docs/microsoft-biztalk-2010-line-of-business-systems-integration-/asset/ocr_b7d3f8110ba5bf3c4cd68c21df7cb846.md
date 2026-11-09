11. Next, create the rules that our services will use to accept or reject the vendor data. Underneath the same scope, create a rule named **ActivityPermission_Create** of type **Simple** and set the **Issuer** to **Riley Semiconductor** with a **Type** of **Group** and a **Value** of **Employee**. What we just did was define an input claim where we are checking if our Issuer (our vendor) gave us a "Group" claim indicating that the service caller is an employee. The output claim type is **ActivityPermission** with a value of **Create**.

![](bbox=[0.33, 0.27, 0.67, 0.54])

12. Define another rule named **ActivityPermission_Remove** that uses a claim type of **Group** but has a value of **SiteManager**. The output claim is of type **ActivityPermission** and has a value of **Revoke**. You can imagine when we have a dozen vendors that being able to map a diverse set of input claims to a single output claim is a powerful concept!

13. Save your changes to the AC STS by clicking the **Save to Cloud** button on the AcmBrowser application toolbar.

What we have done here is define a set of rules that our services can now leverage when confirming that the service caller is authorized to perform the action requested.