There are two types of state machines:

+   - **Standard workflows** – Standard workflows execute exactly once and can run for up to 1 year. Each step in a standard workflow will execute just once. These are ideal for long-running workflows.
+   - **Express workflows** – These are designed for high-volume transactional workloads such as streaming data or IoT data ingestion. These workflows can run for up to 5 minutes.

State machines can be started using various services, such as API Gateway, EventBridge, Lambda functions, or even manually. In the next section, you will learn about the different states in a state machine.

# What Are States?

AWS Step Functions uses ASL to help you create a template to define your state machines, with a collection of states such as task states (to perform some work), choice states (determine which state to transition to), and fail states (if it needs to stop execution with an error). ASL is based on the **Javascript Object Notation (JSON)** structured language.

States within your state machine can perform a variety of functions. Each state can receive an input, perform a certain action based on the input, and then pass out an output to other states. These are the available states:

+   - **Succeed state** – This is when a state completes an execution.
+   - **Fail state** – This stops the execution of a state machine and is marked as a failed state.
+   - **Wait state** – This is where the state will wait for a specific period or a specific date and time. It pauses the processing of the state machine for a specific time or until a specific date and time.
+   - **Choice state** – This is where the state machine can choose between different paths depending on an input. For example, you might want a different output depending on whether the credit card payment was accepted or declined in an e-commerce application.
+   - **Parallel state** – This allows you to add separate branches of execution depending on some input. A choice state can lead to a parallel state. So, for example, if the payment process is completed successfully, then you may wish to initiate multiple actions across parallel branches.
+   - **Map state** – This state accepts a list of things, such as a list of orders. You can run a set of steps against each item in that list.
+   - **Task state** – A single unit of work carried out by the state machine. It can be an activity; an invocation of a Lambda function; integration with another service such as DynamoDB, SNS, or SQS; or calling an external API. The state machine itself doesn’t do any work; it simply uses Task states, which integrate with other services to perform the actual work.

Next, we review the process of designing workflows with Step Functions.