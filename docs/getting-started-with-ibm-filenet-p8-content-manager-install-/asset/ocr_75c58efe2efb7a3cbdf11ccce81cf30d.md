One of the properties on the workflow subscription object is a reference to a Workflow Definition object. WorkflowDefinition is a subclass of Document in which the content is an encoded workflow definition. A workflow definition is usually created via an application called Process Designer (PD). PD is not included with the basic CM license, but two pre-defined workflow objects are included and can be easily accessed from XT.

The Fixed Approval Workflow consists of three pre-defined workflow steps for reviewing, approving, and optionally publishing a document. It's called "fixed approval" because the steps are pre-defined. The Sequential Approval Workflow lets you define a flexible number of steps for the approval process. In both cases, XT provides an interface for defining the participants for each step and other details for each use of the workflow.

# AddOns

We earlier described how to create custom classes and properties. That's a powerful feature for establishing an application-specific data model that follows object oriented principles. It's pretty straightforward to create those classes and properties manually using FEM, but that leaves you with the problem of how to re-create those classes and properties in the deployment environment.

Because everything you can do with FEM can also be done via CE APIs, one solution would be to write a custom application to programmatically create custom metadata in a target Object Store. That idea is appealing because it puts you in complete control of the process. There are some disadvantages, including:

+   - It's another custom application to write and maintain. Depending on your enterprise or other business requirements, this can mean an installation program, application documentation, and other consumability details.
+   - As needs change, the application must be updated. This is a nuisance, especially if the application has to keep track of multiple versions of the custom metadata.
+   - Although it's reasonably documented, the programmatic process of creating or updating metadata is among the lesser traveled roads in the CE APIs, and it's common to see application programming errors in this area.