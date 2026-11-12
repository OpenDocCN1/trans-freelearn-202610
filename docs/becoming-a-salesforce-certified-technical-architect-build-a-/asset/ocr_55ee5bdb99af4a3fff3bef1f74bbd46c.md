# Justifying and Defending Your Solution

We will now move on to the Q&A. Relax and be proud of what you have achieved so far. You are very close to achieving your dream.

During the Q&A, you should also expect the judges to change some requirements to test your ability to solution on the fly. This is all part of the exam. Next, you will see some example questions and answers.

The judges might decide to probe for more details on one topic, such as the proposed solution for IoT. The question could be as follows:

> In your proposal, the tracking devices would send their data to Heroku. You also proposed developing a custom solution on top of Heroku to handle the theft incident. Can you further explain the rationale behind this, and will your solution change if you know that PPA has a plan to introduce logic for several other incident types, such as geofencing or reaching a particular mileage limit?

Avoid losing your composure and assuming the worst straight away. The judge could have simply missed parts of your presentation and wanted to make sure you understand the platform's limitations. Or they could simply be trying to put you under pressure to see how you respond.

Ensure that you fully understand the question. If you do not, ask them to repeat it. Take a few seconds to organize your thoughts if needed. Your answer could be as follows:

> PPA has 5,000 cars, each with an always-on tracking device. The device sends a message every 30 seconds. That is around 14 million messages per day. Salesforce has governor limits for both the number of concurrent calls (although that is mainly for calls with a duration of 20 seconds or more) and the number of API calls per day.

> In addition, considering that Salesforce is a shared-tenancy SaaS platform, an excessive number of Data Manipulation Language (DML) operations could result in degradation in the overall org-wide processing performance.

> I thought of extending the existing PPA global tracking system, but it was reported to be difficult and expensive to change. Therefore, I thought of using a PaaS platform that can scale to that huge API demand. I preferred to use Heroku over AWS because I wanted to use Heroku Connect, which does not consume Salesforce's API limits.

> I understand your concerns regarding developing a custom solution on Heroku. If I had enough time, I would have searched for an existing application with the desired capabilities that can be hosted on Heroku. But I am confident that custom-developed solutions can be created with enough flexibility to accommodate future use cases.