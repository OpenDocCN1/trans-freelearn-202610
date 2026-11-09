an integral part of Salesforce ISV development that can significantly influence the compatibility and adaptability of your solution.

# Dependency injection pattern – a flexible approach to customization

The **dependency injection (DI)** pattern is a popular software design pattern that facilitates customization and extensibility in Apex applications. This pattern allows developers to decouple different components of an application, making it easier for customers to modify specific parts of the app without affecting its core functionality.

For example, imagine you have a managed package application that integrates with an external system to fetch data. By default, your application connects to a publicly available cloud-based off-the-shelf system, but one of your customers may require integration with their in-house system. Using the DI pattern, you can create an interface that defines common methods for interacting with external systems, then develop the default class for the cloud-based system and empower your new customer to develop a new local class for their in-house system. Both classes implement the same interface, so this way, customers can choose which system they want to integrate with, without modifying the core application code.

Another example would be an app that calculates taxes for customers. Different customers may have varying tax rules and calculations. By implementing the DI pattern, you can create a tax calculation interface, and then develop separate classes for each customer’s specific tax rules. This