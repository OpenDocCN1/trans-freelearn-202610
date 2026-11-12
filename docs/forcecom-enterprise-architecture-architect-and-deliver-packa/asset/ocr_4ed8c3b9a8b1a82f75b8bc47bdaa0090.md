# # Application Selector Layer

Apex is a very expressive language to perform calculations and transformations of data entered by the user or records read from your Custom Objects. However, SOQL also holds within it a great deal of expressiveness to select, filter, and aggregate information without having to resort to Apex. SOQL is also a powerful way to traverse relationships (up to five levels) in one statement, which would otherwise leave developers on other platforms performing several queries.

Quite often, the same or similar SOQL statements are required in different execution contexts and business logic scenarios throughout an application. Performing these queries inline as and when needed, can rapidly become a maintenance problem as you extend and adapt your object schema, fields, and relationships.

This chapter introduces a new type of Apex class, the Selector, based on Martin Fowler's Mapper pattern, which aims to encapsulate the SOQL query logic, making it easy to access, maintain, and reuse such logic throughout the application.

The following aspects will be covered in this chapter:

+   * Introducing the Selector layer pattern
+   * Implementation design guidelines
+   * The Selector class template
+   * Implementing the standard query logic
+   * Implementing the custom query logic
+   * Introducing the Selector factory