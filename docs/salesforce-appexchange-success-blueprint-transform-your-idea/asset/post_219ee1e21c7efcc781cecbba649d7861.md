
keeping up with changes in the wider technology ecosystem that your application interfaces with, ensuring your application stays secure, performant, and maintainable in the long term.

## Types of technical debt in AppExchange development

Technical debt in AppExchange development manifests in various ways, each with unique challenges and solutions. We can categorize technical debt into a few categories:

- **Design or architecture debt:** Design debt occurs when shortcuts taken during the design phase become burdens in the long term. For example, if your solution initially stored monthly summary information on a single record, it may have seemed efficient at first, preserving data storage. But as the demand for more fields per month grows, this design choice can quickly evolve into significant technical debt. Such scenarios call for periodic evaluations and necessary redesigns to mitigate such debt.
- **Code debt:** This form of debt emerges from the code base itself. It can be the result of poorly written code, a lack of sufficient documentation, or deviating from Salesforce development best practices. Regular code reviews, sticking to Salesforce’s coding standards, and using static code analysis are some of the ways to manage code debt.
- **Consistency debt:** Often in the pursuit of adopting the latest technologies or methodologies, development teams can introduce a mix of different design patterns, frameworks, and coding styles. For instance, an application that has a haphazard blend of Visualforce, Aura, and LWC can become a nightmare to maintain and debug. While it might be tempting to improve or modernize certain parts of an application, ensuring consistency throughout is crucial. A consistent and well-structured use of Aura, even if considered older, can be more maintainable and efficient than a disjointed combination of the latest tools and styles.
- **Defect debt**: Sometimes, resolving bugs or defects may be postponed due to imminent deadlines. While this might seem a reasonable trade-off at the time, the cumulative effort to address these deferred issues can build up over time, leading to significant technical debt. It’s important to allocate time for bug fixing and not let these issues pile up.
- **Infrastructure debt**: Given Salesforce’s frequent updates to its development tools and platforms, it’s important to stay current to prevent infrastructure debt. For instance, if your **continuous integration/continuous deployment (CI/CD)** process is inadequate, it can slow down your software development life cycle and become a source of technical debt. A well-organized, updated infrastructure and efficient deployment process are essential for preventing this kind of debt.
- **People or skills debt**: The loss of knowledge or skills due to staff turnover can lead to significant technical debt. For example, if the only team member who understood the security review process leaves the company, a re-review request from Salesforce could reveal a serious knowledge gap. To manage this type of debt, it’s crucial to invest in continuous training, comprehensive documentation, and knowledge sharing within the team.

With an understanding of the types of technical debt specific to AppExchange development now under your belt, you are equipped with the knowledge to recognize and mitigate its occurrence in your ISV journey. However, as complex as it might seem, technical debt is not always the villain of the piece. When managed effectively, it can be an unexpected ally. So, let’s shift our perspective and learn how to use technical debt to our advantage tactically.

## Tactical accrual of technical debt

It may sound counterintuitive, but not all technical debt is bad. Sometimes, accruing technical debt can be a tactical decision, made to achieve certain goals more quickly.

Technical debt can be seen as a tool that allows you to move faster in the short term. In a competitive business environment, speed is often critical. Perhaps there’s a market opportunity that requires you to act swiftly or a customer demand that needs an immediate response. In such cases, it might be a wise decision to accrue some technical debt to get a product or feature out the door quickly.

The important thing to remember is that such decisions should be taken consciously and with an understanding of their implications. This implies having a plan for repaying the debt as soon as possible. The faster you “repay” your technical debt by refactoring or improving the code, the less “interest” you accrue in the form of additional time, effort, and resources spent on maintenance and troubleshooting down the line.

However, you should be cautious about when and how much technical debt you decide to take on. Taking on too much debt can slow down the pace of development and make the system increasingly difficult to maintain. It can also lead to a decrease in the quality of the product and affect the satisfaction of your customers.

Remember, the key is not to completely avoid technical debt – that would be nearly impossible – but to manage it effectively. This requires a clear understanding of the trade-offs involved, and the discipline to address the debt before it becomes overwhelming.

## Technical debt can drive your sales

It might seem overwhelming to confront the concept of technical debt: the potential complexities and costs involved might give you pause. But here’s a unique perspective – technical debt can be your ally. Understanding and effectively managing technical debt is one of the reasons why customers might choose your AppExchange app over building their own solution.

Put yourself in the shoes of your potential customer’s **Chief Information Officer (CIO)**. Whenever they face the “build versus buy” decision, they need to consider the implications of technical debt. They have to evaluate the costs associated with its management, and the requirement of maintaining a development team to handle it.

This is where the true ethos of Salesforce and the AppExchange comes to the fore: you manage the technical debt and innovation so that your customers don’t have to.

As a CIO, the AppExchange provides an opportunity to bring innovative capabilities to users faster, while simultaneously reducing the burden of technical debt. Of course, the debt hasn’t really disappeared; it’s simply been transferred from the customer to the ISV. This transfer frees up the customer’s IT team to focus on delivering high-value, innovative solutions for their business at an unprecedented speed.

## Impact on periodic security re-reviews

![](img/44ed632038957e4c80220676e84c5109_538_0.png)

Remember when we dived into security reviews in Chapter 6? It was as exhilarating as your first deep-sea dive into the ocean of code. Now, it’s time to take a refreshing plunge again. Salesforce, much like your diligent credit monitoring service, conducts routine security re-reviews. But here’s the thing: if your code base has a higher technical debt than a shopaholic with a credit card on Black Friday, these re-reviews start feeling like unexpected credit score checks that always bring bad news.

Now, picture this. You open your email, and it’s like getting a letter from your bank saying your credit score needs a review in 3 months. This is easy peasy if your development team was lounging around with feet up, whistling “Sweet Georgia Brown.” But we both know that’s as likely as finding a unicorn in your vegetable patch.

They’re weaving through an ever-evolving roadmap, much like navigating through a city where all the street names are in a foreign language. They’re juggling new customers, akin to handling live grenades (figuratively speaking, of course), and ticking off promised features for existing customers as if they were in a game show with a ticking clock.

But just like maintaining a healthy credit score, managing technical debt isn’t a one-time spring cleaning effort – it’s more of a regular housekeeping chore. It’s as routine as brushing your teeth in the morning. Incorporate these tasks into your everyday process, and suddenly, the re-review will seem as ordinary as your monthly credit score check, not a ghost haunting your development process.

## Early warning signs of technical debt
