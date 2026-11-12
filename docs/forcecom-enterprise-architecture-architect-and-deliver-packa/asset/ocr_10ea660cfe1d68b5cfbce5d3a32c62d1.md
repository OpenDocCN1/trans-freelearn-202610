As code and Visualforce pages contained within extension packages make reference to other Custom Objects, fields, Apex code, and Visualforce pages present in base package, the platform tracks these dependencies and the version of the base package present at the time the reference was made.

When an extension package is installed, this dependency information ensures that the subscriber org has the correct version (minimum) of the base packages installed before permitting the installation to complete.

You can also manage the dependencies between extension packages and base packages yourself through the Versions tab or XML metadata for applicable components (we will revisit versioning in Apex in a later chapter when discussing API integration).

# Package dependencies and uploading

Packages can have dependencies on platform features and/or other packages. You can review and manage these dependencies through the usage of the Package Detail page and the use of dynamic coding conventions, as described here.

While some features of Salesforce are common, customers can purchase different editions and features according to their needs. Developer Edition organizations have access to most of these features for free. This means that as you develop your application, it is important to understand when and when not to use those features (this is done in order to avoid unwanted dependencies that might block or inhibit the adoption of your application).

By default, when referencing a certain Standard Object, field, or component type, you will generate a prerequisite dependency on your package, which your customers will need to have before they can complete the installation. Some Salesforce features, for example Multi-Currency or Chatter, have either a configuration or, in some cases, a cost impact to your users (different org editions). Carefully consider which features your package is dependent on.

Most of the feature dependencies, though not all, are visible via the View Dependencies button on the Package Detail page (this information is also available on the Upload page, allowing you to make a final check). It is good practice to add this check into your packaging procedures to ensure that no unwanted dependencies have crept in. Clicking on this button for the package that we have been building in this chapter so far confirms that there are no dependencies.