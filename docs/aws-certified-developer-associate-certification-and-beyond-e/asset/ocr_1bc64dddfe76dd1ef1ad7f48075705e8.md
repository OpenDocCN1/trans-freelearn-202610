# Real-Time Stream Processing

With services such as Amazon Kinesis or DynamoDB Streams, Lambda functions can process streaming data in real time, which is useful for analytics and log processing. The following diagram provides a flowchart of an AWS event-driven integration model, showing the journey of an event from creation to the execution of a Lambda function and potentially triggering a notification service.

![](bbox=[0.17, 0.21, 0.83, 0.41])

Figure 8.1 – Lambda event-driven architecture

The following are the key components of this architecture:

+   - **Event Producer:** This represents the event’s origin, such as a user action or system occurrence that generates an event. It can be an application, service, or a manual process.
+   - **Event:** The actual occurrence that initiates the process. It’s a signal that something has happened that needs to be processed by the system.
+   - **Amazon EventBridge Event Bus:** This service receives the event from the producer. The EventBridge Event Bus acts as a central router for events, directing them to the appropriate rules and targets based on the event pattern.
+   - **Rule:** The criteria set within EventBridge that matches incoming events to the correct processing logic. Rules can filter and transform events, ensuring only relevant data triggers the Lambda function.
+   - **AWS Lambda:** A serverless computing service that runs your code in response to events. AWS Lambda is triggered to execute the associated function when the specified rule is met.
+   - **Amazon Simple Notification Service (SNS):** An optional step in the flow where a Lambda function may invoke SNS to send notifications based on the event. SNS is a managed service that provides message delivery from publishers to subscribers (such as email, SMS, or to another Lambda function).