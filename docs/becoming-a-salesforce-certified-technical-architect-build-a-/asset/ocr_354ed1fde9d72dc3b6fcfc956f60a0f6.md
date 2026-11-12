# # Car Status Update and Penalty Settlement

PPA has tracking devices in its cars; they are used to track the car’s location by periodically sending its GPS coordinates. The devices also send additional information such as incidents of harsh braking or overspeeding.

What do you call devices equipped with sensors and other technologies that can communicate with other devices and services over the internet to exchange data? Correct, internet of things (IoT) devices.

There are specific authentication flows explicitly designed for IoT devices, such as the device flow and the asset token flow (which is more suitable for IoT devices that are not equipped with a screen). You covered both in Chapter 4, Core Architectural Concepts: Identity and Access Management.

IoT devices tend to send small chunks of data at a high frequency. This typically results in generating a massive amount of data considering the number of connected IoT devices at any given time.

As a rule of thumb, the Salesforce Core Cloud (also known as Force.com) is not an ideal platform to receive such a massive number of inbound communications, nor the best place to store the retrieved data over a long period.

In some scenarios, you might still be able to pitch Salesforce Core for this role. But in that case, you need to carefully calculate the expected inbound calls and compare them with the likely governor limit at the target org. A safer approach would be to utilize an off-platform solution to receive and aggregate the result. Salesforce Heroku is an excellent fit for such scenarios, especially with some add-on products such as Apache Kafka on Heroku, Postgres, and Redis.

Read the shared requirements and start crafting your solution. The first requirement is as follows:

*All tracking devices can communicate over 4G and 3G. They send the car’s location every 30 seconds.*

PPA has 5,000 cars (and it is fair to assume that this number will grow). The tracking devices send a message every 30 seconds; this takes place day and night for security reasons.

That means two messages every minute, 120 every hour, and 2,880 every day. In total, that would be 14,400,000 daily messages from all cars. That is over 430 million messages every month.

Clearly, Salesforce Core is not the ideal platform for such a massive number of call-ins or create/update operations. But you still have to do the math to justify your rationale. This is precisely what you are expected to do in your real-life project.

You are an architect, and you cannot simply assume that there will be many call-ins. Based on that, I propose the following solution. Make reasonable assumptions and do the required math; numbers are the best way to deliver an unquestionable message.