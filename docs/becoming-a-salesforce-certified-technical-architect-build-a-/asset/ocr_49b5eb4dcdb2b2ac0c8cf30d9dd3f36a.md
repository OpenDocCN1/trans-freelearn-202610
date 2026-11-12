**Note**

So far, you do not have any requirement indicating that PME’s distributors will have access to Salesforce. Therefore, you can assume that *Pricebooks* will continue to be accessible to all sales agents. To adjust this solution to work with partner communities, you need to update the OWD of the *Pricebook* object to no access or view only, then share the right *Pricebook* with the right set of users.

Update your data model diagram and move on to the next requirement.

*In the negotiations stage, the distributor should be able to place an order, which contains a list of the device models and quantities.*

You now have a requirement indicating the need for distributors to access Salesforce. The distributors will need the ability to create orders. They will also require a user license that supports record-sharing capabilities using manual sharing or sharing rules. Both Partner Community and Customer Community Plus provide these capabilities. However, it is common to utilize the Partner Community licenses with distributors (after all, they are business partners). You can assume that and adjust later if needed. Your proposed solution could be the following:

*To grant the distributors access to Salesforce to place orders, I propose assigning a Partner Community license to their users. I will update the OWD value of the Pricebook object to no access. The admin will then manually share each distributor’s Pricebook with the right partner role for that distributor.*

Update your landscape architecture, data model, role hierarchy, and actors and licenses diagrams. They could now look like this:

![](bbox=[0.19, 0.58, 0.83, 0.85])

Figure 9.3 – Landscape architecture (second draft)