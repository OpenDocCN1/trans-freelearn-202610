# Which implementation method is best?

Each implementation method has its own merits and may be suitable depending on a number of factors. As architects, it is our job to advise according to the goals and strategic initiatives of the organization, giving consideration to maintainability.

# What has any of this got to do with Salesforce?

Now we know what MDM is, and the theory behind how to implement it, let's turn our attention to MDM and Salesforce. Typically, Salesforce is implemented to bring together data from a company's enterprise in order to break down information silos and provide a consistent view of the data across various business units or departments. Salesforce is therefore a de facto source of truth and immediately becomes the data master when implemented in such scenarios. Our single source of truth would therefore exist in Salesforce.

Salesforce almost always needs to communicate data changes with one or more systems. Therefore, implementing a middleware system is typically used to facilitate the communication and data field mapping between connected systems that constitute our golden record in Salesforce. As part of the Salesforce system design, the produced data dictionary for the objects and fields created in Salesforce can be passed to the middleware team in order to facilitate the data mapping in order to broker data updates between Salesforce and connected systems.

The initial data migration exercise to Salesforce should be used as the point in which the single source of truth for a given record is constructed from the source systems, cleansed, de-duplicated, and enriched as required or appropriate prior to loading into Salesforce and then turning on the integration for changes to only be enacted in connected source systems after the go-live date.

Now we know what MDM is, let's explore the concept of the single source of truth in more detail.

# The golden record

As we now know, the **golden record**, or **single source of truth**, is essentially a data record where the definitive source of information is stored in order to orchestrate business operations. Users should be able to view a golden record to see the correct version of a piece of information. While the definitive source of information for users may well be the golden record, integration of associated systems may be responsible for the updates to that data, or pushing golden record data updates to those associated systems, as explained in the previous section.