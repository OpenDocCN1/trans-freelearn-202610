
In the next chapter, *Setting Up Teams for Success*, we will cover some ideas that foster collaboration, learning, and growth. We will also focus on the benefits of a no-blame feedback culture.

## Further reading

To learn more about the topics that were covered in this chapter, take a look at the following resources:

- Common cloud security issues and threats: https://www.strongdm.com/blog/cloud-security-issues-risks
- Workshop for threat modeling for builders: https://catalog.workshops.aws/threatmodel/en-US

## Part 4

## Taking the Next Steps

This part focuses on setting up your teams for success, both from a cultural and technology standpoint. It dives into the typical problems enterprises face with the increasing adoption of AWS and offers practical tips to address them, while increasing team collaboration and streamlining developer processes. You will learn about some strategies for continuous improvement and adherence to the AWS Well-Architected Framework. Through the concept of landing zones, we will additionally introduce you to some open source solutions and frameworks. These frameworks can be combined with AWS services such as Control Tower for improved compliance and security of your multi-account, multi-Region environments.

This part has the following chapters

- Chapter 10, Setting Up Teams for Success
- Chapter 11, Ensuring a Strong AWS Foundation for Multi-Account and Multi-Region Environments
- Chapter 12, Adhering to AWS Well-Architected Principles

## Setting Up Teams for Success

In the DevOps realm, the success and effectiveness of all initiatives depend on the coordinated integration of both people and technology. Focusing on just one is not going to reap many benefits. By recognizing the importance of both, organizations can unlock the full potential of DevOps, resulting in faster software delivery life cycles with increased quality and reliability.

**People** drive the cultural and collaborative aspects of DevOps. They bring their skills and expertise to drive a mindset of shared responsibility, continuous improvement, and effective communication. **Technology**, on the other hand, provides the necessary tooling, automation, and infrastructure management to support this mindset. Software delivery approaches such as CI, CI/CD pipelines, and **Infrastructure as Code (IaC)** empower teams to improve the overall efficiency of delivering code to the customers.

With DevOps, people reach for tooling too quickly. Before they do that, they need to ensure the right foundations of – culture and working model. Once people are aligned with the value and benefits of both, they will be motivated to adopt the required technology changes and ways of working by themselves. In this chapter, we will focus on the two pillars – people and technology – and what you can do in the AWS world to set up your teams for success. Additionally, I will introduce you to some resources that you can leverage for continuous learning and enablement of your DevOps teams. Adoption of AWS often opens up new opportunities to innovate and sometimes even leads to role transitions within the company. Toward the end of this chapter, we will cover a basic learning path that you could consider for yourself or other team members who would like to get more involved in DevOps on AWS.

Our agenda for this chapter is as follows:

- Building a collaborative and agile team setup and culture
- Technology best practices and considerations for success
- Resources to support AWS learning and development
- Learning path for transitioning into DevOps roles

## Building a collaborative team setup and culture

Adopting DevOps practices is easier said than done. A fundamental problem that most organizations struggle with is looking at tools and automation as the golden solution to all their problems – or even worse, hiring DevOps engineers from the market and embedding them into existing teams, hoping that they will introduce automations and cloud services that will magically address all gaps. Before going this route, they first need to ensure *people readiness*. In this section, we will discuss the various aspects of building an effective team structure that supports successful DevOps implementation across your organization. We will focus on the importance of fostering a culture of collaboration and effective communication that, in turn, drives value creation for customers.

## Enable your teams to create more value

The first challenge you need to tackle is your company’s operating model. How are the teams structured, what are their responsibilities, how do they communicate, and how well does this structure align with the business goals? Does it allow them to independently deliver business outcomes to the customer? Is there a need for frequent hand-offs to other teams for routine day-to-day work? Do they lack expertise in areas that are critical to the functioning of their software, and so on?

Four foundational team structures can be adopted by organizations operating at any scale. Before we dive into the details, it’s important to highlight that these team setups might take slightly different forms, depending on the size and DevOps maturity of the organization. So, instead of applying them as a static template, consider them as inspirations from which new teams could be formed, or existing teams could transition into. Focus more on what value these structures bring and how they could help your teams focus on what they do best.

### *Value-stream aligned teams*

These teams are laser-focused on a well-defined unique stream of work. They are your front-line warriors, always having well-established communication interfaces with the end customers, external or internal. The north star for this team should be providing business value to the end user. Depending on the company size and product offering, this single stream could be scoped to a complete product or service, a collection of features within a product, or something more granular if needed. What is important is that they take complete responsibility, starting from gathering inputs from the customers, understanding their pain points, developing software to address the gaps, and operating it in production. Companies such as **AWS**, for example, might want to align these team structures with different services that the cloud provider offers. So, a dedicated team might take care of **AWS CodeBuild**, and another could own **AWS CodePipeline**, for example. These teams have possess cross-functional skills that allow them to understand the business needs of the customers and translate those needs into software features. They should also have all the skills in-house for operating these workloads at a global scale. From a DevOps perspective, this checks all boxes, such as you-build-it-you-run-it, operating with autonomy, building with the end customer in mind, and so on.

Taking a different example of, let’s say, **eBay**, might warrant the need to scope the streams a bit differently. So, there could be a team responsible for *checkout*, one for *product catalog* listing, another for *fraud management*, and so on. However, the core principles remain the same. These teams should be able to deliver value almost independently of any other team in the company.

Another challenge organizations frequently face is how big these teams should be. To ensure effective collaboration, shared responsibility, and team dynamics, it’s a good rule of thumb to keep the headcount below 10-12. This is what Amazon also calls the *2-Pizza team*, a team that can be fed with two pizzas and has the full authority to make decisions in the best interest of their stream.

The other teams we are going to discuss are more *enablers* for the core value streams in your organization. They are the ones coming into the picture to offer support in areas that slow down the value streams in providing value to the end users.

## Platform teams

The platform teams are focused on developing and maintaining reusable services for their customers – the individuals in the different value streams, across the organization. The sole success criteria for the platform teams should be the ease of adoption, usage, and documentation of the services they provide. The more compelling and easier to use these services are, the wider the adoption.

In *Chapter 1, Accelerating Your DevOps Journey with AWS*, we discussed the idea of reducing cognitive load for customers. The concept of a platform team strongly relates to this idea. They should enable the value stream members to be able to focus on differentiating business outcomes. This is only possible by offloading other undifferentiated tasks from them. The outcomes from the platform team address the common needs of all other value streams. For example, if all the software artifacts depend on containerized workloads running **Amazon Elastic Kubernetes Service (Amazon EKS)**, then the right focus area for the platform team should be ready-made templates for deploying, monitoring, and operating services in these EKS environments. They could offer centralized dashboards and out-of-the-box logging integrations for the value streams to easily adopt and benefit from.

However, it is important to note that ownership of the production environments of software developed by respective value streams still sits with them.

In *Chapter 11, Ensuring a Strong AWS Foundation for Multi-Account and Multi-Region Environments*, we will discuss the concept of a **Landing Zone**. This maps well to the scope of work that a platform team would ideally deal with, in the context of AWS environments. This enables the application teams to focus on what they do best while leaving the complexities around the foundational aspects of a multi-account, multi-region environment with the central platform team.
