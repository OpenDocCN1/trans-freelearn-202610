# Considerations for implementing a compliant data model

The Salesforce Certified Data Architect credential syllabus talks about implementing a **compliant** data model in the context of data governance. As Salesforce Data Architects, it is on us to ensure that we can recommend an approach for designing a data model that conforms to the **compliance standards** that are set out in regulations such as the following:

+   - **General Data Protection Regulation (GDPR)**, European Union
+   - **California Consumer Privacy Act (CCPA)**, United States
+   - **Personal Information Protection Act (PIPA)**, Japan
+   - **Lei Geral de Proteção de Dados (LGPD)**, Brazil
+   - **Personal Information Protection and Electronic Documents Act (PIPEDA)**, Canada

This includes going over the various options available for identifying, classifying, and protecting sensitive data.

We'll look at one such regulation now, namely the GDPR.

# What is the GDPR?

The GDPR is the European data protection law that defines how personal information regarding EU individuals needs to be handled.

The GDPR is there to control how information is used by organizations or governments. Individuals have a legal right to know how their information is collected, processed, and stored. They have a right to request the information a company, government, or entity holds about them, and a right to be forgotten. If such a request is made, the organization holding the data is required to delete it, subject to the fair processing of data (which the person must have been made aware of when they freely opted into the company or entity collecting this information). Companies are legally required to collect information fairly, only use it for the specific purposes they need it for, store it securely, and erase it when it's no longer needed. If a company falls foul of the GDPR, the greater figure out of a maximum of €20 million or 4% of their annual global turnover is what the fine could be (this is according to the GDPR itself).

The impacts of complying with the GDPR from a software application perspective include ensuring that the application data model can record consent and privacy preferences. As we already know, Salesforce includes several standard objects to assist with recording preferences and consent (such as the Individual object). Additional custom objects can be added for specific requirements.

# The Individual object

According to the Salesforce documentation available at https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_individual.