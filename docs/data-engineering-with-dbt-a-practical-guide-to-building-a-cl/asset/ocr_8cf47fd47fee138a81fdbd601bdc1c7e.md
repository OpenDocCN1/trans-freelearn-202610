It is common to refer to the following three types of data models, which differ in the level of detail:

+   - **Conceptual data model**: This is the most abstract model, defining *what will be in the domain of the project*, providing the general scope
+   - **Logical data model**: This model provides much greater detail, defining *what the data will look like*
+   - **Physical data model**: This is the most detailed model, describing exactly *how the data will be stored in the database*

In the following subsections, we will describe these three types of models.

# Conceptual data model

The conceptual data model is the least technical of the models, as it is used to *capture the most important entities and relations at the business level.*

This model identifies the core master data entities and their relations, defining what will be in the domain of the project, providing the general scope and naming.

A good starting point for drawing it is to identify all the entities that are needed in the desired reports, clarifying the semantics of the entities so that they become clear to all people involved in the project.

This model must use real business entities and relationships that are relevant for the real-world business and usually does not contain attributes or other technical details, such as system-specific names or attributes.

It is created between data architects and business stakeholders and should be complemented by a collection of the most important business rules and constraints.

Because a conceptual data model describes the real business as it is in reality, with real-life entities, it is expected to be valid across different projects that touch on the same domain.

# *Example of a conceptual model*

The following conceptual data model uses the UML notation to provide a simple, business-level data model of orders and returns:

![](bbox=[0.245, 0.73, 0.795, 0.875])

Figure 3.2: Conceptual data model of orders and returns