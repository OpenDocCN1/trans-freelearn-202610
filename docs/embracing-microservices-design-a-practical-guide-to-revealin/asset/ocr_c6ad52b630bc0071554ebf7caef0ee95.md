# Telemetry

In a cloud-native environment, teams have less control over the execution environment. They are dependent on the capabilities provided by the cloud vendor. Installing debuggers and inspectors is not an option, which makes real-time application monitoring and telemetry a challenge. To address telemetry, you need to make sure that you are collecting system/health data, application performance data, and domain-specific data so that you have complete visibility of the system in near real time.

# Security

Security is an important pillar of a well-architected cloud-native application. The cloud-native application is built to run in multiple data centers to serve customers across the globe. They need to ensure that the clients accessing these applications are authenticated and authorized. These applications should identify every requestor and grant access based on their roles. At times, these requests are encrypted using **Transport Layer Security (TLS)** to ensure data privacy while data is in transit. Teams also need to think about securing their data at rest with the capabilities available in different storage platforms. As an example, Azure Cosmos DB and an Azure SQL server allow us to encrypt data at rest.

The preceding 12 factors and the three additional factors we've looked at here have given us a thorough understanding of building cloud-native microservices with an API first mindset. In the cloud-native world, security and telemetry are no longer an afterthought. Approaching security with a zero-trust mindset is essential, while continuously collecting data to monitor system health gives you the ability to make the necessary adjustments to achieve higher availability for the systems.

# Summary

In this chapter, we learned about various aspects of setting the mindset for starting your microservices journey. We dissected the microservice architecture into its components and understood the design principles for building microservices. Adopting microservices has several benefits, which essentially help organizations bring business value faster to their end users. We also discussed the challenges of adopting microservices and how these can be addressed.