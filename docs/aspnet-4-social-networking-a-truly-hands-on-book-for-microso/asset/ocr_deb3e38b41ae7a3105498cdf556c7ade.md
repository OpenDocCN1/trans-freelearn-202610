+   - **Extensibility:** We will try to build the greater part of the application in extensible manner so that if needed to we can replace parts of the functionality without having to recompile the entire application or even redesign.
+   - **Build versus Buy:** Wherever possible we will try and utilize out-of-box functionality in the .NET stack or look for third party/open source components that can help fill the gap.

Each of the above principles is discussed in the following section. As we go along, we will also see how they can be realized with the help of technology and architectural patterns.

# Layered architecture

Knowing that our application is going to be a large undertaking that should last for some time, we need to design something that can be easily extended as the need arises. For that we will try to design this application with extensibility in mind. The easiest way to do that is to follow the general rule of maintaining "separation of concerns (SoC)".

# Layers

The easiest way to maintain SoC is to first break major areas of your application into layers. In most applications, this might be broken down into the presentation, business logic, and data access layers.

+   - **Presentation Layer:** This layer would normally hold anything pertaining to the user interface of your application—the buttons, links, and other controls that a user would click on and interact with while using your application.
+   - **Business Logic Layer:** This is where the rules of your application would live. This could be as simple as formatting the currency of a price in your product catalogue to something more complex such as enforcing rules regarding data input.
+   - **Services Layer:** While building **Service Oriented Architecture (SOA)**, services layer is built on top of the business logic layer. Since we are not aiming SOA implementation we will not create this layer. However, in Chapter 13, we will show you how to introduce this layer in the existing design without a lot of changes.
+   - **Data Access Layer:** The data access layer is responsible for connecting to a data source and interacting with the data that is stored in that location. This could be a database, some XML files, text files, or even a web service.