
At this point, most companies haven’t been running AI/ML programs for decades and don’t have dedicated teams. Tech companies like Meta, Amazon, Apple, Netflix, and Google are leading the cultural norms with managing AI/ML, but most companies that will need to embrace AI are not in tech and are largely unprepared for the technical debt that AI adoption will pose for their engineering teams to manage. Managing and integrating legacy systems alongside new AI technologies and capabilities, building structure around the AI/ML life cycle, bringing in enough technical skill to make it all happen in the first place, scoping compute resources and storage needs, managing cloud and on-premises services and infrastructure, and managing costs are all complexities an AI PM will need to think about and prepare for.

Shortcuts taken to get AI initiatives off the ground will require code refactoring or changing how your data is stored and managed, which is why strategizing and planning for AI adoption is so crucial. Even beyond the planning and costs, an AI PM will need to help the organization measure the success and failure of the AI initiatives it’s planning, particularly if those AI initiatives are directly supporting their product. Setting up performance metrics, measuring the impact on the business, establishing a pattern of adoption and satisfaction, and linking that to return on investment (ROI) are all important facets of an AI PM’s work. This is why so many of these IaaS services are popping up to help keep engineering teams nimble should they require changes in the future as well. The infrastructure needed to keep AI teams up and running is going to change as time goes on, and the advantage of using an IaaS provider is that you can run all your projects and only pay for the time your AI developers are actually using data to train models.

## Deployment strategies — what do we do with these outputs?

Once you’re happy with the models you’ve chosen (including their performance and error rate) and you’ve got a good level of infrastructure to support your product and chosen AI model’s use case, you’re ready to go to the last step of the process and deploy this code into production. Keeping up with a deployment strategy that works for your product and organization will be part of the continuous maintenance we outlined in the previous section. You’ll need to think about things such as how often you’ll need to retrain your models and refresh your training data to prevent model decay and data drift. You’ll also need a system for continuously monitoring your model’s performance so this process will be really specific to your product and business, particularly because these periods of retraining will require some downtime for your system.

> Over time, an ML model’s performance can experience a gradual reduction in performance if there are changes to underlying patterns or relationships in the data that is being used to train them. This is known as model decay.
> 
> When data used by an ML model changes or shifts from what the model was originally trained on, it can make a model’s predictions less accurate because the patterns have changed. This is known as data drift.

For example, if an e-commerce company wants to try out a new pricing algorithm to see if it would have an impact on sales, it could split users into two groups. One group would see the old pricing model while the other would experience the new pricing algorithm. Since these two groups would reflect real pricing and purchasing decisions, the results could be compared between the two over a period of time. This would allow the company to monitor the site and compare sales performance, user engagement, and conversions between the two groups of customers to be able to understand the real impact of the new pricing algorithm.

## Canary deployment strategy

Here, we see a more gradual approach to deployment where you actually create subsets of users that will then experience your new model deployment. Here, we’re seeing the number of users that are subjected to your new model gradually increasing over time. This means that you can have a buffer time between groups of users to understand how they’re reacting and interacting with this new model. Essentially, you’re using varying groups of your own users as testers before you release to a new batch so you can catch bugs more gradually as well. It’s a slow but rewarding process if you have the patience and courage.

For example, if a photo-sharing social media app wanted to test out a new photo editing feature, it might only deploy that new feature to a small test group. In this case, the “canary” group. They could monitor the performance, adoption, and feedback from this small group to identify any bigger issues the feature or capability might have before they roll it out to their entire user base. Being able to test a feature with a smaller group minimizes potential risks they might otherwise have with a wider rollout. After the success of that initial experiment, they can have a chance to make changes or improvements from the feedback before opening it to a wider audience.

There are more strategies to choose from but keep in mind that the selection of these strategies will depend on the following:

- The nature of your product
- The budget, which is what’s most important to your customers and users
- Your metrics and performance monitoring
- Your technical capacity and knowledge
- The timeline

Beyond deployment, you’re going to have to help your business understand how often they should be doing code refactoring and branching as well.

## Example

Let’s take an example of a food delivery service that was looking to launch a new feature for an app that improved delivery time predictions. They considered their options between canary, A/B testing, and shadow deployment approaches with the following pros and cons:

- **Shadow deployment:** Running the new delivery time feature along with the current delivery time feature. The new feature would gather data about customer interactions but the current delivery time feature would be the only one actually outputting delivery times visibly to users. Let’s look at the pros and cons of this approach:
  - **Pros:** The product team can test the new feature with real conditions without it actually affecting users.
  - **Cons:** Because users won’t actually interact with the new delivery time feature, the product team won’t actually know how they’d be affected by it and if it would impact user engagement or user behavior.
- **A/B testing:** The product team would split users into two groups with the first group using the current delivery time feature and the second using the new delivery time feature. The pros and cons would be as follows:
  - **Pros:** The product team can compare how effective the current and the new feature would be at predicting delivery times, as well as how they impacts the user, their behaviors, and their engagement.
  - **Cons:** Testing the features needs to be carefully considered and designed in a way where the two groups can be equitably compared. The product team also needs to be careful not to accidentally overlap users.
- **Canary:** The product team could test out the new delivery time feature with only 5% of its users and monitor their reaction to it. The following are the pros and cons:
  - **Pros:** This allows the product team to monitor any adverse reactions to user behavior and engagement and fix any issues before more users experience them.
  - **Cons:** The small group of users might not be reflective of their entire customer base. Some user segments that aren’t in the canary group might be more sensitive to the new feature.

In the end, the product team decided to go with the *A/B testing deployment* strategy because they didn’t want to risk unknowns from their canary group and they also didn’t want to push a feature without any users directly experiencing the change like they would in the shadow deployment strategy. They wanted to get clear, comparative measures of the impact of the new feature on user engagement, user behavior, and delivery accuracy all at once. Seeing how the rollout would impact users in real-world scenarios and having a direct comparison to the existing feature was paramount. Because the company had the infrastructure to handle A/B testing appropriately, as well as the means to analyze the results comprehensively, they went ahead with an A/B test of their delivery time prediction feature.

Now that we’ve discussed the different deployment strategies, let’s take a bird’s eye view and see where AI is taking us.
