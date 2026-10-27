
Data modeling is generally used in two main contexts:

- To describe what data should be generated or collected by an application or in a project. In this case, we build the data model before having the data.
- To describe some data that already exists. In this case, we reverse-engineer the data model from the available data.

This distinction is crucial because in the first case, you have the freedom to design a model how you need it to be according to the needs of the use case, while in the second case, the data is given, that is, it already exists, and consequently, the data model can only reflect how the data is, with no real freedom of modeling.

In data warehousing, we always start with the second case, as we get data “as is” from source systems, and if we do not have a data model of the source system, we reverse-engineer one that describes the data to the best of our knowledge.

Usually, we have multiple source systems, with quite different data models, even if they contain similar data, and a goal to integrate data from them. The typical situation is that none of the models from any of the source systems are easy to understand for the analysts nor ideal to answer the business needs.

We therefore end up transforming the source data to build a new target data model that should be simpler to understand and satisfy the business needs as much as possible and makes it possible to combine the desired data from the different source systems.

A good practice is to explicitly design this new data model before coding the transformations, drawing at least one model that represents the business concepts that are the goal of the integration and will form the refined targets of our transformations. This model can be at a high level of abstraction, describing the business and only the main entities, or at a greater level of detail, already representing the technical design.

This is different from the situation in the first case because building this new target data model is a constrained exercise, and the modeler should keep in mind the starting models because the further the new data model lands from the sources, the greater the need for extra information, such as mapping tables and master data, and work to transform the data and the higher the risk of losing or altering the original data, introducing errors.

Having an explicit target model simplifies the collaboration and provides a clear goal for the project. It is also a good starting point to analyze whether we are missing any information or need mapping tables or business rules to go from the source system data sets to the desired target model.

This target model can also be interpreted as the set of all the business concepts and their relations that are needed to power the reports in the desired data marts. In fact, the best way to identify the concepts that should be in the target data model is by going backward from the desired reports to the business concepts.

> **Note**
> We will often refer to data marts and reports, as that is the most common use of data marts, but consider that a data mart is just a way to provide a well-thought-out set of tables to satisfy a use case, be it a report, an AI/ML model, exporting data, or something else.

The next step is to identify the needed connections from these concepts to the data in the source systems, identifying when we are missing some connection or change keys and need mapping tables, business rules, or master data.

## Complementing a visual data model

We have seen that a data model is a communication tool, and the concise nature of a visual data model makes it a great tool to convey some technical information, while other important aspects for data engineering projects, such as the semantics or business rules, are not clearly expressed using only the standard components of a visual data model.

We therefore suggest complementing the visual data model with other kinds of documentation that can better convey these kinds of information.

Descriptive documentation, such as a wiki or a glossary, is a great way to explain the semantics of the entities and attributes that appear in your data model, build alignment on the meaning of the terms that you use in the model, and explain important business rules that are vital to be understood besides the technical implementation details.

Another important aspect is to properly describe how we derive the data in the new destination data model starting from the data sources. This is called data lineage.

When you use dbt, it is easy to keep this information close to the code, and some information, such as the data lineage, is automatically generated from the code itself.

Now that we know what data models are, their purpose, and why they are useful, it is time to dig into more detail. In the next section of this chapter, we will introduce the different types of data models, and in subsequent sections, we will look at notations to express them and some examples and problems. We will close by looking at different modeling styles and architecture that frame the use of data models.

## Conceptual, logical, and physical data models

Data models can be designed with slightly different notations, but no matter how you design them, a model that describes everything in your data project would be as complex as your database and become too big to be useful as a communication tool.

Furthermore, when working on a data engineering project, you have discussions with different people, and these discussions focus on different levels of detail with respect to the data, business, and technical aspects of the project.

It is common to refer to the following three types of data models, which differ in the level of detail:

- **Conceptual data model**: This is the most abstract model, defining *what will be in the domain of the project*, providing the general scope.
- **Logical data model**: This model provides much greater detail, defining *what the data will look like*.
- **Physical data model**: This is the most detailed model, describing exactly *how the data will be stored in the database*.

In the following subsections, we will describe these three types of models.

### Conceptual data model

The conceptual data model is the least technical of the models, as it is used to *capture the most important entities and relations at the business level.*

This model identifies the core master data entities and their relations, defining what will be in the domain of the project, providing the general scope and naming.

A good starting point for drawing it is to identify all the entities that are needed in the desired reports, clarifying the semantics of the entities so that they become clear to all people involved in the project.

This model must use real business entities and relationships that are relevant for the real-world business and usually does not contain attributes or other technical details, such as system-specific names or attributes.

It is created between data architects and business stakeholders and should be complemented by a collection of the most important business rules and constraints.

Because a conceptual data model describes the real business as it is in reality, with real-life entities, it is expected to be valid across different projects that touch on the same domain.

### Example of a conceptual model

The following conceptual data model uses the UML notation to provide a simple, business-level data model of orders and returns:

![](img/8868d272a75eee8b407a8e0f3434d10f_130_0.png)

Figure 3.2: Conceptual data model of orders and returns

Despite its simplicity, this model already opens up discussions with the business and captures some interesting information, such as whether there is a data connection between returns and orders or not, or whether a return can be related to only one order or even multiple orders.

While it is true that we might figure out this info by analyzing the source data, it is also true that digging out everything from data is slow and error prone, also because business rules such as these change over time.

Having both the data and the expected rules from the business allows us to spot inconsistencies before they reach the reports and the users.

### Logical data model

The logical data model is a technical model that defines how the data will be organized, without yet committing to a particular technical implementation.

This model starts to identify the key attributes of the entities, such as **Natural Keys (NKs)** and **Business Keys (BKs)**, which will be used to identify the entity instances or to connect to other entities or reference tables. It also identifies key measures and categorizations that are important for the use of the data.

This model also includes operational and transactional entities, supporting tables, and mapping and reference tables as needed, depending on the part of the domain that is in focus.

It is usually developed between the data architects and business analysts to provide a detailed map of what needs to be developed and how it will work. It must be complemented with all the non-trivial business rules and data structure specifications.

When a good conceptual model exists, this model can be built and refined incrementally as the project approaches the different areas of the domain.

### Example of a logical model

The following logical data model is drawn using the crow’s foot notation, which is the simplest and most common for E-R diagrams. The model represents only the order part from the example in the conceptual data model:
