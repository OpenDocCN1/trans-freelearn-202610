# # Introducing Salesforce objects

In this section, we will look at the foundational data structure in Salesforce – objects. This includes looking at what Salesforce objects are, their properties, and their usage. I will be suggesting best practices and principles to keep in mind when working with both standard and custom objects and other types of objects available on the platform. Applying these best practices when working with objects will help us keep our organizations scalable for that next enhancement or major project and deliver increased usability for our end users. The intent here is to briefly review the different objects and field types in Salesforce before we go into a detailed discussion of data modeling. This will make it easier to understand and grasp the data model concepts and provide a good foundation as regards when to use the different relationship types.

We will start with standard objects, which are the most commonly used and familiar to us, and then review other objects in Salesforce as well.

# ## Standard objects

These are objects that come out of the box with Salesforce. Common ones include Account, Contact, Opportunity, and Lead, to name a few. Your license type and the version of Salesforce you are using will define which ones you can use in your organization.

# ## Custom objects

These are objects created to support the business requirements of your company. Examples include the Job, Applicant, and Candidate objects that we looked at in our recruitment app in the last chapter. AppExchange applications will also have custom objects in them.

# ## Big objects

This is a unique object because it can hold massive volumes of data from hundreds of millions of rows to billions of rows. They are well suited for archiving data or bringing in data from external sources. However, they are also limited compared to standard and custom objects. Some of the key features of big objects that differentiate them from regular objects are given here:

+   - They can only be created using metadata.
+   - They are prefixed with __b.
+   - They cannot have triggers, nor can they be reported on.