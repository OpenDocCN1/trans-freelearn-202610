# Salesforce Data Management

With a grasp of **Master Data Management (MDM)** concepts under our belt, we can now look at Salesforce Data Management. In this chapter, we'll look at how the Salesforce data model and Salesforce licensing work together. The license types chosen for a solution have an impact on the objects available. This can extend to record sharing facilities available in the case of Experience Cloud license types.

We'll look at what data persistence looks like on the Salesforce Platform and how that works when governing solutions on the Salesforce Platform.

Next, we'll expand on our *golden record* theory from *Chapter 3, Master Data Management,* to explain what a **Single Customer View (SCV)** looks like on the Salesforce Platform, and how data from multiple Salesforce clouds and multiple Salesforce instances can be pieced together.

Therefore, in this chapter, we'll cover the following topics:

+   - Analyzing Salesforce license types and access to objects
+   - Persisting data
+   - Representing a single view of the customer on the Salesforce Platform
+   - Pulling together data from multiple Salesforce instances

# Analyzing Salesforce license types and access to objects

In this section, we're going to cover how the different Salesforce licensing options affect the objects and standard functionality available to users. Given the vast array of functionality available in Salesforce products, it is important to ascertain what the required objects and functionality will be (from both an internal and external access perspective) so that the correct licenses can be purchased. As expected, certain licenses are more expensive than others—for example, Service Cloud licenses are more expensive than Sales Cloud. In the case of Experience Cloud, this type of license not only affects the objects available for users but can affect sharing rules and role hierarchy too. We'll get to those implications further along in this section.