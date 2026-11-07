
Conversely, a managed or fully managed service is a commercial solution that provides support for most of these tasks. Fully managed machine learning services provide out-of-the-box support for the following:

- *A managed infrastructure:* When deploying NLP services, the final model will likely live on a particular machine somewhere in a data center. In order for our users to use our service, they will need to have means of reaching this machine through a domain name or an IP address. To set up a self-serve NLP service, infrastructure needs to be put in place to make sure that the right requests hit the right server so that it can return the results to the user. All these decisions are taken care of for us as part of a managed service.
- *Managed endpoints:* A machine learning model, from an engineering perspective, is a binary file that can be loaded into memory and, through a machine learning library, perform inference. However, given that we are using a server-side inference approach, it means that if a user needs some predictions from an NLP model, they will need to send a request over the internet. This request will need to reach the service, which will need to interpret the request and respond appropriately with results. Therefore, there needs to be a means of communication agreed by the client and the service. A popular solution to this is using a communication protocol called **REST**, which allows communication through a **REST API** that utilizes the **HTTP** or **HTTPS** protocol. But the machine learning endpoints are not just responsible for client-server communication, parsing requests, performing inference, and returning the results to the user. They have a number of other responsibilities. They need to return appropriate error messages if something goes wrong (for example, if the input text exceeded the length limit). They need to return appropriate error messages if the user doesn't have access. They need to be able to handle a large number of requests at the same time and queue appropriately. They need to perform optimization tasks such as caching.

When building a production-grade NLP service, you will need to think about these issues and implement solutions yourself. Managed services take care of all that.

But it's worth mentioning that managed services can only do so much. Self-serving, while cumbersome, allows you to implement whatever you want and add any feature you can think of. With managed services, you are limited to using only the features offered by the service.

- *Scaling*: One of the biggest selling points of fully managed ML services is the promise of scaling. It resolves one of the biggest issues with self-serving, which is getting caught up in a non-scalable infrastructure. For example, let's imagine you implement a simple ML endpoint on a single virtual machine. This solution will be easy to implement and work great for a few daily users. But when you start getting more users, the service will start returning delayed results, and too much traffic will sooner or later cause a denial of service. The application will then need to be modified to be able to handle a medium amount of daily users. By the time this solution is implemented, you will have lost a great deal of customers due to service unavailability. When a solution capable of dealing with a medium amount of traffic is implemented, you may realize that the number of users is increasing too quickly and that the entire solution will need to be reimplemented again to make it high-traffic capable.

Fully managed ML services claim to solve all these problems. They advertise their services as scalable by default. This means that if your service works great for 1 daily user, it should, in theory, work the same for 1 million daily users. This allows you to spend time and resources on more useful endeavors.
- *Deploying models and versioning*: Most fully managed ML services come with a set of tools and features for deploying ML models. In some cases, using them is as easy as uploading the ML model binary and clicking **deploy**.
- *Monitoring*: If you decide to self-serve, you will inevitably also need to build or set up tools for monitoring. These tools provide insights about how many users are using the service and help you decide whether or not you should scale. Nearly all managed services offer this out of the box.
- *Freemium pricing plans and low setup costs*: Most fully managed ML services have a near-zero setup cost and a pricing plan where you pay only for what you use. Furthermore, some services offer a freemium service where the first *N* daily/monthly requests are free. Pricing for these services is often competitive and affordable compared to the spending resources with self-serving. It is only when a company grows to a significant size (such as becoming a big corporation) that a fully managed service will become less economical than hiring a dedicated team responsible for self-serving.

## Important Note

Fully managed ML services also feature tools for the complete ML life cycle, which includes data labeling, data preprocessing, model training, deployment, and validation. These features are, by design, limited in terms of what they can do and often only support a limited set of NLP tasks, such as text classification, but they may not work for tasks such as sequence tagging or specifically training Flair models.

Because few popular managed ML services offer support for the entire ML life cycle of all the types of models that Flair can build, we plan to focus only on the steps required to get your NLP model deployed to production. The steps required for setting up automation pipelines for maintenance and model refresh will be left out, as they relate to a more general field called MLOps.

The preceding bullet list shows a number of points clearly working in favor of fully managed services. This begs the question, why not use a managed service over self-serving every time?

The general answer to the preceding question is: if you can, you should. There are, however, some cases where self-serving may be a more reasonable choice. You can decide to self-serve if the following applies:

- You want to learn about how setting up ML services works from a low-level engineering perspective.
- You have a large enough user base that makes using managed ML services uneconomical.
- The ML library, framework, or task you are working on isn't supported by the managed service.

Unless you fall under any of the categories described in the preceding bullet list, you are likely to be better off simply using a fully managed service.

With this, let's have a look at what services you can use to deploy Flair models.

## Managed services for Flair models

Managed services for hosting ML models are usually commercial solutions offered by either big-tech cloud platforms or specialized ML-oriented companies that offer ML as a service.

The list of services that can be used with Flair, however, is constantly changing, and so are the user interfaces for interacting and managing these services. This means that a step-by-step guide focusing on how to interact with the UI or API of a specific managed ML service is likely to become outdated within a year.

However, you're not out of luck! There is one exception. There is one specific service that is tightly integrated with Flair and is likely to be supported for years to come. It's called the **Hugging Face Models Hub**. It's a repository that hosts a vast number of ML models, as well as providing tools to test them in a web browser, viewing instructions on how to train them, and deploying them as a service.

Later on, we'll have a look at how to train, host, and use Flair models deployed as part of the Hugging Face Accelerated Inference API service.

But before wrapping this section up, let's recap all the advice, tips, and tricks covered in this section.

## A model deployment decision-making flowchart

To make Flair model deployment decision-making easier, we prepared a flowchart that helps you decide on the right path to deploying Flair models to production.

Depending on your business needs, resources, and aspirations, you will have these three options for deploying your Flair model:

- Client-side deployment with local inference
- Fully managed ML as a service solution
- Self-serving
