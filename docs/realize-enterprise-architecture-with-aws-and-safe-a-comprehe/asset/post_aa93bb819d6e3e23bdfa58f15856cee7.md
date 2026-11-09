
## Planning

In order to migrate resources to AWS, you must figure out how you will migrate them, the tools you will use, and the operating model you will use to maintain, govern, and secure them. Prepare for the migration by following these best practices:

- Identify the key assumptions for a comprehensive wave plan. The number, dependencies, and tools used in each sprint, along with the owner of each sprint, should be identified.
- Integrate account structures, security, networking, automation, and pipelines.
- Preparing the landing zone is essential.
- The migration tools must be selected and obtained.
- Define the processes for change management and impact assessment.
- Decide how you will communicate.
- Establish the production processes and operating models.
- Resources, including partners, must be selected and committed.

## Activities

All sprint weeks should be planned in advance. Members of the team should be involved in every sprint week! Bear in mind the following:

- To monitor progress and guide team members, create key metrics. Define what and how each objective should be measured.
- Plan for risks by conducting a premortem.
- Decide how everyone will be kept informed and set a schedule.
- Identify and improve sprint activities by holding an inception retrospective.
- The backlog of products should be reprioritized.
- Plan how you will continue to practice each day.
- Organize a retrospective to identify areas for improvement.
- Stay on schedule at all times.

The sprint, planning, and retrospective meetings should also be set up on a shorter cadence. A two- to three-week sprint offers rapid feedback, allows you to pivot, and creates urgency. The Wave 1 migrations consisted of developing blueprints and building blocks and testing specific migration patterns. Developing a migration factory process and expanding agile models for your entire application portfolio is now your next step as you build a team to support your initial wave of migrations.

Migration factories enable large-scale moves to AWS by paving the way. An efficient migration workflow combines the technical, business, and human elements of cloud migration. Cloud migrations have been disappointing in several cases for their teams. Despite their failures, they stuck with it and improved their results with follow-up attempts. When you're planning to migrate mission-critical applications to the cloud as part of your organization's modernization, you can't afford to repeat mistakes made by others. To maximize your chances of successful cloud migration, this post uses those learnings to craft a 10-step checklist of the most important factors you need to consider. The following sections are the components of the cloud migration checklist.

## Step 1 – establishing the migration-architect role

You should establish the role of the migration architect before you begin your cloud migration. Migration architects play a pivotal role in planning, implementing, and closing out all aspects of migrations; their primary responsibilities include defining the refactoring strategies necessary to facilitate migration success, developing the migration strategies for data migration, defining the cloud-solution requirements, and determining the migration priorities and switchover mechanisms.

When a migration project is large, there are many technical decisions to make, and the presence of a migration architect who can address all of the aspects of the project is essential to the success of the project.

## Step 2 – choosing your level of cloud integration

A shallow cloud integration or a deep cloud integration can be used when moving an application from a data center to the cloud. When you use shallow cloud integration, you move the on-premises application to the cloud without changing anything about the servers you use to run the application in the cloud. It just takes a few changes to make the application run in the new environment. The services are not unique to the cloud. Lift-and-shift is another term for this model, in which the application is transferred to the cloud intact.

You modify your application to take advantage of key cloud capabilities during the migration process for deep cloud integration. A serverless computing capability such as AWS Lambda for portions of the application might be as sophisticated as auto-scaling and dynamic load balancing. Using a cloud-based data store such as Amazon S3 or DynamoDB might be another option.

## Step 3 – choosing a single cloud or going multi-cloud

If you plan to migrate your application to the cloud, you should ask yourself the following question: are you planning to run your application on a single cloud provider, or are you planning to run it on multiple providers? Cloud provider-specific application optimization is relatively straightforward. You only have to learn one set of cloud APIs, and your application will be able to take advantage of all the features the cloud provider has to offer. This approach has the downside of vendor lock-in. Your application might require just as much effort to migrate to another cloud provider when you have already updated it to work with that one provider. Furthermore, you might not be able to negotiate key terms with your cloud providers, such as pricing and SLAs, if you have one cloud provider.

A cloud contains one application, and another cloud contains another application. Using multiple clouds might seem straightforward at first; one set of applications runs in one provider and another set in another. The advantage of this method is that you can leverage multiple providers to increase business leverage and flexibility when it comes to where to put new applications. Additionally, the applications can be optimized for the provider on which they run. Using multiple cloud providers can help you scale your application. Several companies choose to run parts of their applications with one cloud provider but others with another. It allows you to take advantage of the key advantages that each provider offers. In this situation, your application might be impacted by either provider’s performance, and any issues with either provider could affect the customer experience.

Cloud-agnostic development is crucial to your application’s success. Those who do not adopt cloud-agnostic development risk losing their customers. Your application could be run simultaneously across several providers, or the load could be shared among them. The model makes it easy for you to easily switch the load between different cloud providers, giving you the best flexibility when negotiating with vendors.

## Step 4 – establishing cloud KPIs

An application’s key performance indicators (KPIs) relate to how the application or service performs relative to the expectations you have for it. Your applications and services might have already been assigned KPIs, but are they still the right ones when they are cloud-based? KPIs are the most useful for understanding the progress of your cloud migration, highlighting any problems that you might not have noticed. A cloud migration KPI can determine when migration is complete and successful, which is perhaps the most important factor. Cloud migration KPIs can be divided into several categories:

| Category | Sample KPI |
|---|---|
| User experience | Page load time |
|  | Lag |
|  | Response time |
|  | Session duration |
| Application/component performance | Error rates |
|  | Throughput |
|  | Availability |
|  | Apdex |
| Infrastructure | CPU usage % |
|  | Disk performance |
|  | Memory usage |
|  | Network throughput |
| Business engagement | Cart adds |
|  | Conversions and conversion % |
|  | Engagement rates |

Assess the importance of each category to your business and determine which metrics will be affected the most by the cloud migration. Using tools such as infrastructure monitoring, you can then track what is happening.

## Step 5 – establishing performance baselines

Baselining involves measuring the current performance of your service or application and determining if its future performance is acceptable. Based on your baselines, you can determine when your migration is complete and how much you can improve your performance post-migration. As you migrate to the cloud, you can also use baselines to diagnose any problems that occur.

Each key performance indicator should have a baseline metric. Set the timeframe for collecting data to establish the baseline. Short baseline periods allow you to move more quickly, but they do not give you a representative sample of your performance. In terms of time, a longer baseline period results in more representative data, but it takes more time.
