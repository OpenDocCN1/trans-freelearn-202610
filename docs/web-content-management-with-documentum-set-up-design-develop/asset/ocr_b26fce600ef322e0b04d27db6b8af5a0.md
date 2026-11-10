# Does relative path (the XML Path Language (XPath) // (double slash)) work in the case of Instruction Files?

No, this does not work and is a known limitation with Instruction Files.

# What is meant by the Automatic Property Extraction (APE) feature in Web Publisher?

Automatic Property Extraction (APE) in Web Publisher is a feature that allows you to automatically set the values of specified XML elements in content XML files based on values provided for content attributes and vice versa.

# Which particular widgets does Automatic Property Extraction work with in Web Publisher?

The following widgets are supported by Automatic Property Extraction (APE) in Web Publisher: Textline, checkbox, and Choice.

# Which object types does Documentum use for managing Automatic Property Extraction (APE)?

Documentum uses the following object types for managing Automatic Property Extraction (APE) in Web Publisher: dm_xml_application (subtype of dm_folder), and dm_xml_config (subtype of dm_document).

# What is the difference between a Workflow process template and a Workflow in Documentum?

A Workflow process template (dm_process object) contains the definition for the workflow process, containing specifications regarding the various activities and their performers, actions performed in the workflow process, and so on and so forth.

On the other hand, Workflows (dm_workflow objects) are run-time instances created from the Workflow process template for routing content.

# What are flows in Documentum Workflows?

Flows connect the various activities in a workflow template and could either take routed objects in a forward direction (in case of approvals) or a backward direction (in case of rejects).

# Mention the two types of Workflow activities.

The two types of activities in Workflows are: Manual and Automatic.

While Manual activities are performed manually by a specified set of groups or users, Automatic activities are automatically performed by the server.

# What are packages with reference to Workflows?

Packages are added to flows connecting various activities in a workflow template. These in turn route the objects in a workflow.

# Mention the different types of publishing operations in Site Caching Services (SCS).

The different types of publishing operations in Site Caching Services (SCS) are: full refresh, incremental refresh, force refresh, and single object publish.