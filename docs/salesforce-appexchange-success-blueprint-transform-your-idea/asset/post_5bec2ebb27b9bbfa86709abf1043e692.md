
## Package extensions – tailoring applications for multiple products and editions

Package extensions offer a flexible approach for AppExchange partners to build applications that cater to customers using different Salesforce products, such as Health Cloud and Financial Services Cloud, as well as different Salesforce editions such as Professional. By utilizing a base managed package and an extension package, you can create a modular application that satisfies diverse customer requirements without maintaining multiple code bases.

Consider your application as a multifaceted puzzle, where the base managed package constitutes the central pieces, forming the core functionality suitable for all customers. Extension packages, on the other hand, are like specialized puzzle pieces that enhance the base package with additional features and functionalities, tailored to specific Salesforce products or editions.

In the context of 2GP, you can create a base 2GP package containing the core features and then develop one or more extension 2GP packages that depend on the base package. In line with the organizational structure and needs, these extension packages can encompass layered functionalities, ranging from the data model, and logic, to the UI. These packages might also incorporate features specific to certain Salesforce products or editions, allowing partners to explore and extend their offerings effectively. This modular approach allows you to maintain a single code base while offering tailored solutions to different customers.

When designing your application and the interfaces between the packages, consider the entire application life cycle. It’s crucial to ensure updates to the base package do not break the extension package. When adding new features, include them in the appropriate package and make the necessary components `@namespaceAccessible` components if they are referenced by the extension package.

Leveraging package extensions in 2GP enables you to create scalable and modular applications that cater to a diverse range of customers while simplifying the upgrade process. As your customers’ needs grow, you can easily upsell them by introducing the appropriate extension package to enhance their application experience.

## Package extensions versus feature parameters

When developing on the Salesforce AppExchange marketplace, you might often find yourself in a dilemma: should you build an extension package or use the **Feature Management App (FMA)**, described in *Chapter 2*? Both approaches have their own advantages and uses. Here’s a handy comparison:

| Criteria | Package extensions | Feature parameters |
|---|---|---|
| Scope | These are designed to extend the functionality of a base package. They have a dependency on the base package and can’t be installed independently. | This is more about controlling the visibility and access to specific features within a package. It doesn’t add new functionality but rather manages existing ones. |
| Flexibility | This approach provides a great deal of flexibility because it enables you to add new objects, fields, logic, and dependencies on Salesforce features that are not required by your base package. It’s like building a new app but with a dependency on the base package. | Limited to toggling features on or off. New components must be added to the base package. |
| Use case | Ideal for scenarios where you have different versions of your app for different industries or when you have add-on modules that not every customer might need. | This approach is ideal for beta testing features or for gradually introducing new features to your customers. It also allows for control over usage allowance allocations and the disabling of features in a freemium or tiered-access model. |
|  | Requires more development and testing effort as you’re essentially building a new (albeit smaller) app. | Easier to implement as you’re just controlling access to existing features. |
| Complexity |  |  |
| Technical debt | Can reduce technical debt due to better **separation of concerns** (**SoC**). | Can increase technical debt due to added complexity in the core code base. |

Feature parameters are particularly suited for gradual feature deployments, gaining insights through A/B testing, allowing end users to customize product functionality dynamically, and ensuring efficient maintenance when enhancements are closely tied to the core product. Importantly, they don’t introduce the overhead of maintaining and releasing separate packages, making them a streamlined solution for managing features within the core package. However, this approach, while offering flexibility in feature evolution, can increase the complexity of the core code base.

Conversely, package extensions are designed to augment the functionality of an existing product by creating separate packages. They are the preferred choice when there’s a need for modular functionality, managing complexities that might interfere with the core product, or when new features demand a different versioning and release rhythm. Package extensions, while promoting better SoC and reducing technical debt due to their modular nature, come with their own challenges. They require creating, maintaining, and releasing separate packages, which can introduce additional overhead. Their compartmentalized design ensures a cleaner code base but demands more rigorous management.

In making a decision, you should weigh the nature of your product, its strategic objectives, and technical challenges. While package extensions are apt for modular and intricate enhancements, feature parameters are adaptable and versatile. The final decision should resonate with your distinct trajectory and goals.

## Dev Hub

Dev Hub is an essential tool for Salesforce developers and AppExchange partners, providing a centralized environment to manage, develop, and deploy Salesforce applications, particularly 2GP managed packages. In this section, we will discuss the relationship between Dev Hub and 2GP managed packages, emphasizing critical information you need to know.

The managed package in a 2GP packaging model is owned by Dev Hub, but the packaged metadata is not stored in it. To use a 2GP package, it is recommended to enable Dev Hub in your **Partner Business Org (PBO)**. In a 2GP packaging model, a single Dev Hub instance can own one or more packages, streamlining the management process for multiple applications.

It’s recommended to register the namespace in a separate Developer Edition org, which can then be linked to your Dev Hub instance. This process allows you to use a namespace and associate multiple namespaces with a single Dev Hub instance, enhancing organization and flexibility. To achieve this, you would need to do the following:

- 1. Create a separate Developer Edition org if you don’t already have one with a registered namespace.
- 2. Register the namespace in the Developer Edition org. It’s important to select namespaces wisely, considering they cannot be changed or reused once associated with an org.
- 3. Log in to your Dev Hub org as a system administrator or as a user with Salesforce DX Namespace Registry permissions.
- 4. From the **App Launcher** menu in your Dev Hub org, select **Namespace Registries**.
- 5. Click **Link Namespace** and log in to the Developer Edition org using the system administrator’s credentials.

Please be aware that before you can link the namespace, it’s necessary to define and deploy a **My Domain** name in your Dev Hub org, if not already done.

## Localization best practices for managed packages

In the dynamic world of Salesforce ISV development, everything should be localized. This includes everything from screen flow text, custom tabs, objects, and fields to exception messages that might be presented to the user.

## Managing redundancy and dependencies

You might come across redundancy in your labels, especially if your app is large. However, it’s important not to worry too much about it. The same phrase may have different connotations in different languages, and what might seem redundant in one language might not be so in another. Therefore, treating each label as unique and giving each its appropriate context can be beneficial.

## Modularizing labels and versioning

Labels should ideally be kept in a single `CustomLabels` XML file per package, maintained in alphabetical order by API name. You should manually incorporate new labels into your package metadata, specifically within the `CustomLabels.labels-meta.xml` file. This entails utilizing an **integrated development environment (IDE)** rather than relying on the Salesforce UI.

Using a prefix on each label API name to define the module it belongs to and categories to group labels together can further help with organization.

## Translation tools

The translation process can be outsourced to a professional translation service. Exporting labels to XLF format can simplify the process for them, and they will maintain a catalog of what they have already translated, making it easier to identify new or modified entries each time.

## Allowing subscribers to adjust your translations

In namespaced packages, end customers can override metadata translations for custom objects. For instance, they can override the label on a custom field or a workflow task using Translation Workbench. If they do so, updates to your application are not going to override local translation. Although you
