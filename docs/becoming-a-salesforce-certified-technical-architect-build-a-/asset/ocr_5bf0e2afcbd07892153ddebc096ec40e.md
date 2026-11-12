+   • Integration technology: Selecting the right technology is essential. Depending on the integration techniques available, there might be higher dependencies on specific skill sets, hardware, or software. This might impact the speed and agility of your project.

+   • Data format: Data that is exchanged between different applications must follow a pre-agreed format. In the enterprise world, this is unlikely. Therefore, the integration process must have an intermediate step where the data is translated from one format into another.

Another related challenge is the natural evolution of data formats. Flexibility to accommodate the changes and extensions of a data format plays a great role in defining the flexibility of the integration architecture.

+   • Data versus functionality: Integration is not necessarily about sharing data. The integrated applications could be looking to share functionality. Think of the use case where one application needs to invoke a particular functionality in another system, such as checking for a particular customer's credit score. There will likely be a set of parameters being sent to the other end to facilitate the logic of the remote process. Invoking remote functionalities can be difficult and could have a significant impact on how reliable the integration is. As a Salesforce Architect, you need to be aware of specific integration patterns, and you need to understand the limitations of the platform. You will cover this in Chapter 9, Forging an Integrated Solution.

There are several considerations that an architect should consider when designing an integration strategy or a distributed solution. Now, you need to understand what key integration styles are available and what they should be used for.

# Introducing Common Integration Styles

When designing an integration architecture between two or more systems, the key challenge is how to achieve that. There are some common integration styles that architects should be familiar with. You need to have basic knowledge of them and understand how and when to use each style. In today's world, some of these integration styles have evolved and are used as part of modern enterprise integration platforms. Next, you will become familiar with the file transfer, unified datastore, remote procedure invocation (RPI), messaging, and mashup integration styles.

# File Transfer

In this integration style, applications produce a file containing the data that other applications would consume. This file is normally in a format that can be read by all the target systems and shared in a repository that can be accessed by all concerned systems. These systems are responsible for transforming the file into any other format they are expecting, while the file's producer is responsible for generating the data files at regular intervals based on business needs.