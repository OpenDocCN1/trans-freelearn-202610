
## Specialist teams

Depending on an organization’s domain of business, there might be certain specialized areas that are overarching across most, if not all value streams. These areas require special skills that are not commonly found in the industry, or within the company. This could be deep expertise in media encoding, financial risk analysis algorithms, high data compression techniques, and so on.

In all likelihood, this knowledge is core to the artifacts that the value streams provide. It is essential to continue providing value to the end customers. In such cases, the organizations can consider establishing these specialist teams who *consult* the value streams on best practices around the niche skillset. Highly mature DevOps companies also invest in offering these specialized skills *as a service*, often abstracting the underlying details with easy-to-understand interfaces, thereby reducing the cognitive load of the value streams.

## Enablement teams

The ever-increasing pressure to deliver more features sometimes derails the value streams from focusing on their core differentiated offering. They might get pulled into tasks that are temporary but require a deep skillset, or experience, which they don’t necessarily have the time to learn.

This pattern frequently surfaces in organizations that, for example, are migrating their applications from one technology stack such as OpenShift (RedHat’s Kubernetes offering) to EKS in AWS. Not all value streams might be aware of the peculiarities of such a migration and the dependencies in their application stack that they should consider evaluating. To spearhead the progress of such migrations, it often helps to set up enablement teams, a temporary structure that is formed of individuals who have been involved in similar migrations, for many other workloads in the company. The experience they have had in the past can be a big value add for the teams who want to do something similar.

A CoP is a group of people within the organization that share a concern or passion for something. They meet regularly to collaborate and learn from each other. These communities might have multiple dimensions of purpose; sometimes, helping other communities with everyday work needs, sharing best practices and guidelines for the organization, or fostering breakthrough innovations and new practices. As shown in Figure 10.2, a CoP is an overlapping structure that spawns multiple teams in the company and gets everyone with similar interests together in a room:

![](img/ecd265dfca9bb64161408ba8a944a4d9_250_0.png)

Organizations typically plan these collaborative working sessions at least once a week, when undergoing major transformations. In later stages, they might want to revisit the cadence by further reducing or increasing it, depending on the value that the teams are driving from the setup.

## Measuring the DevOps maturity of your teams

An important prerequisite to improving something is to start measuring it. The same applies to the DevOps maturity of your teams. It is essential to identify the areas of improvement, understand progress, and track the effectiveness of any process or change rolled out as part of DevOps practices in your organization. But what exactly needs to be measured, you may ask?

**Accelerate 2022 State of DevOps Report** (Ref: `https://services.google.com/fh/files/misc/2022_state_of_devops_report.pdf`) mentions two clusters of analysis, one targeted toward *software delivery performance*, and the other toward *operational performance*. These reports are based on inputs from more than 33k respondents from different organizations. *Figure 10.3* highlights the four key metrics in the software delivery performance category that will help you quantify your team’s current performance and measure it against the industry benchmark:

| Software delivery performance metric | Low | Medium | High |
|---|---|---|---|
| **Deployment frequency**<br>For the primary application or service you work on, how often does your organization deploy code to production or release it to end users? | Between once per month and once every 6 months | Between once per week and once per month | On-demand (multiple deploys per day) |
| **Lead time for changes**<br>For the primary application or service you work on, what is your lead time for changes (i.e., how long does it take to go from code committed to code successfully running in production)? | Between one month and six months | Between one week and one month | Between one day and one week |
| **Time to restore service**<br>For the primary application or service you work on, how long does it generally take to restore service when a service incident or a defect that impacts users occurs (e.g., unplanned outage or service impairment)? | Between one week and one month | Between one day and one week | Less than one day |
| **Change failure rate**<br>For the primary application or service you work on, what percentage of changes to production or released to users result in degraded service (e.g., lead to service impairment or service outage) and subsequently require remediation (e.g., require a hotfix, rollback, fix forward, patch)? | 46%-60% | 16%-30% | 0%-15% |

Figure 10.3 – Four metrics to measure software delivery performance

(source: Accelerate 2022 State of DevOps report)

It gives you an idea of how other companies are doing and if you’d like to aim higher by improving your current standings in any way.

However, all of this needs to be discussed and planned against the real needs of your application and the expectations of the end customers. So, context matters. Not all teams and applications require 10 deployments per day, but if they do, it’s necessary to make sure that your release process support it.

The same report further describes a second clustering approach that accumulates the metrics from software delivery performance and sprinkles the reliability aspect on top of it. A holistic view of all these areas gives a representation of the current state of the workloads that you are managing, resulting in four different categorizations:

- Starting
- Flowing
- Slowing
- Retiring

As a DevOps practitioner, your goal should be to ensure that the result of this analysis matches the expectation from the workload or the value stream. These categorizations are highlighted in *Figure 10.4:*

| Cluster | Stability: Time to restore service | Stability: Change failure rate | Operational Performance: Reliability | Throughput: Lead time | Throughput: Deployment frequency | % respondents |
|---|---|---|---|---|---|---|
| Starting | Between one day and one week | 31%–45% | Sometimes meet expectations | Between one week and one month | Between once per week and once per month | 28% |
| Flowing | Less than one hour | 0%–15% | Usually meet expectations | Less than one day | On demand (multiple deploys per day) | 17% |
| Slowing | Less than one day | 0%–15% | Usually meet expectations | Between one week and one month | Between once per week and once per month | 34% |
| Retiring | Between one month and six months | 46%–60% | Usually meet expectations | Between one month and six months | Between once per month and once every 6 months | 21% |

Figure 10.4 – Clustering software delivery and operational performance metrics to identify the current state of the workloads (source: Accelerate 2022 State of DevOps report)

## De-silo Dev and Ops

Many organizations might divide their teams into separate functional areas, leading to groups consisting of developers, operations, QAs, and so on. This model keeps you away from the benefits that shared responsibility, ownership, and team cohesion can bring on your DevOps journey. The whole idea of the DevOps methodology is to break down these silos, ensure teams speak more often, are aligned on what the end customer needs, and collaborate on achieving this goal. The wall that exists between these teams creates resistance to change that is detrimental to reliably releasing software more often. Developers who have had some system administration background in the past should actively involve themselves in improving the operations of the software. Operators, on the other hand, might be well versed in some application programming languages, so they could leverage those capabilities to assist the developers in reliably rolling out changes at scale.

However, introducing this change in organizations that have since long been using these siloed models might face some resistance. A strategy that I’ve seen work quite well in such teams is to temporarily *embed* DevOps practitioners into the teams who can act as agents of change. By introducing new practices around development, operations, and communications backed by automation and tooling, they can motivate the team members to think beyond their preset boundaries and focus on driving customer value.
