In terms of monitoring, by default, Lambda Functions produce a **CloudWatch** Logs stream and standard metrics. You can access log information by navigating through your Lambda Function monitoring section and clicking on *View logs in CloudWatch.*

In CloudWatch, each Lambda Function will have a **Log group** and, inside that Log group, many **Log streams**. Log streams store the execution logs of the associated function. In other words, a log stream is a sequence of logs that share the same source, which, in this case, is your Lambda Function. A log group is a group of log streams that share the same retention, monitoring, and access control settings.

We are reaching the end of this section, but not the end of this topic on Lambda Functions. As we mentioned earlier, this AWS service has a lot of use cases and integrates with many other services. In the next section, we will take a look at another AWS service that will help us to orchestrate executions of Lambda Functions. These are known as **AWS Step Functions**.

# Working with Step Functions

Step Functions is an AWS service that allows you to create workflows in order to orchestrate the execution of Lambda Functions. This is so that you can connect them in a sort of event sequence, known as **steps**. These steps are grouped in a **state machine**.

Step Functions incorporates retry functionality so that you can configure your pipeline to proceed only after a particular step has succeeded. The way you set these retry configurations is by creating a **retry policy**.

> **Important note**
> Just like the majority of AWS services, AWS Step Functions also integrates with other services, not only AWS Lambda.

Creating a state machine is relatively simple. All you have to do is navigate to the AWS Step Functions console, then create a new state machine. On the *Create state machine* page, you can specify whether you want to create your state machine from scratch, from a template, or whether you just want to run a sample project.

AWS will help you with this state machine creation, so even if you choose to create it from scratch, you will find code snippets for a variate list of tasks, such as AWS Lambda invocation, SNS topic publication, running Athena queries, and more.