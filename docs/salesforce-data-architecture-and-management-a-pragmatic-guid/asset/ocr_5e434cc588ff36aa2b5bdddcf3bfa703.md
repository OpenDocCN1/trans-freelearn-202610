# The Universal Data Dictionary (UDD)

The physical database at Salesforce looks very different from what you see when you look at an object's metadata in **Setup** or run a `describeSObject` method from the backend. This is because Salesforce employs an abstraction layer in the application server, which, at runtime, reads data from the database and applies other logic resulting in the shape of your organization, in other words, objects, fields, relationships, list views, and so on. This layer, called the **Universal Data Dictionary**, or **UDD** for short, is declarative, meaning that object names, their attributes, and relationships are stored and not generated through code. What you see in the org is the result of the kernel acting on the UDD since the kernel acts as a mapping layer between the UDD and what you see in your org. The following is a diagram of the UDD and its components:

Shared Database
Pivot Tables
Metadata Tables (UDD)
Data Tables

Metadata Cache
Bulk Data Processing Engine
Runtime Application Generator
Query Optimizer
Full-Text Search Engine

Tenant-Specific Screens
Common Application Screens
Objects (Tables)
Virtual Application Components at Runtime