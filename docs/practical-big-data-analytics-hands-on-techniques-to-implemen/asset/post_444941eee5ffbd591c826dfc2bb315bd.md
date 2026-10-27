
In this case, *effort* is the time and work required to implement the use case. This includes aspects such as how long it would take to procure the relevant hardware and/or software that is part of the solution, the resources or equivalent *man-hours* it will take to implement the solution, and the overall operational overhead. An open source tool might have a lower barrier to entry relative to implementing a commercial solution that may involve lengthy procurement and risk analysis by the organization. Similarly, a project that spans across departments and would require time from multiple resources who are already engaged in other projects is likely to have a longer duration than one that can be executed by the staff of a single department. If the net effort is low enough, one can also run more than one exercise in parallel as long as it doesn’t compromise the quality of the projects.

- **Leveraging the early-wins:** The successful implementation of one or more of the projects in the early-wins phase often lays the groundwork to develop a bigger strategy for the big data analytics platform that goes far beyond the needs of just a single department and has a broader organizational-level impact. As such, the early-win serves as a first, but crucial, step in establishing the value of big data to an audience, who may or may not be skeptical of its viability and relevance.

## Implementation life cycle

As outlined earlier, the implementation process can span multiple steps. These steps are often iterative in nature and require a trial-and-error approach. This will require a fair amount of perseverance and persistence as most undertakings will be characterized by varying degrees of successes and failures.

In practice, a Big Data strategy will include multiple stakeholders and a collaborative approach often yields the best results. Business sponsors, business support and IT & Analytics are three broad categories of stakeholders that together create a proper unified solution, catering to the needs of the business to the extent that budget and IT capabilities will permit.

## Stakeholders of the solution

The exact nature of the stakeholders of a big data solution is subjective and would vary depending on the use case and problem domain. In general, the following can be considered a high-level representation of this:

- **Business sponsor:** The individual or department that provides the support and/or funding for the project. In most cases, this entity would also be the beneficiary of the solution.
- **Implementation group:** The team that implements the solution from a hands-on perspective. This is usually the IT or Analytics department of most companies that is responsible for the design and deployment of the platform.
- **IT procurement:** The procurement department in most organizations is responsible for vetting a solution to evaluate its competitive pricing and viability from an organizational perspective. Compliance with internal IT policies and assessment of other aspects such as licensing costs are some of services provided by procurement, especially for commercial products.
- **Legal:** All products, unless developed in-house, will most certainly have associated terms and conditions of use. Open source products can have a wide range of properties that defines the permissibility and restrictiveness of use. Open source software licenses such as Apache 2.0, MIT, and BSD are generally more permissible relative to GNU GPL (**General Purpose License**). For commercial solutions, the process is more involved as it requires the analysis of vendor-specific agreements and can take a long time to evaluate and get approved depending on the nature of the licensing terms and conditions.

## Implementing the solution

The final implementation of the solution is the culmination of the collaboration between the implementation group, business beneficiaries, and auxiliary departments. The time to undertake projects from start to end can vary anywhere from 3-6 months for most small-sized projects as explained in the section on early-wins. Larger endeavors can take several months to years to accomplish and are marked by an agile framework of product management where capabilities are added incrementally during the implementation and deployment period.

The following screenshot gives us a good understanding of the concept:

![](img/8ededb1e0b69e85687f6e488d1044b79_44_0.png)

High level image showing the workflow

The images and icons have been taken from:

- https://creativecommons.org/licenses/by/3.0/us/
- Icons made by Freepik (http://www.freepik.com) from www.flaticon.com is licensed by CC 3.0 BY
- Icons made by Vectors Market (http://www.flaticon.com/authors/vectors-market) from www.flaticon.com is licensed by CC 3.0 BY
- Icons made by Prosymbols (http://www.flaticon.com/authors/prosymbols) from www.flaticon.com is licensed by CC 3.0 BY
- Vectors by Vecteezy (https://www.vecteezy.com)

## Technical elements of the big data platform

Our discussion, so far, has been focused on the high-level characteristics of design and deployment of big data solutions in an enterprise environment. We will now shift attention to the technical aspects of such undertakings. From time to time, we’ll incorporate high-level messages where appropriate in addition to the technical underpinnings of the topics in discussion.

At the technical level, there are primarily two main considerations:

- Selection of the hardware stack
- Selection of the software and BI (**business intelligence**) platform

Over the recent 2-3 years, it has become increasingly common for corporations to move their processes to cloud-based environments as a complementary solution for in-house infrastructures. As such, cloud-based deployments have become exceedingly common and hence, an additional section on on-premises versus cloud-based has been added. Note that the term *On-premises* can be used interchangeably with **In-house**, **On-site**, and other similar terminologies.

![](img/8ededb1e0b69e85687f6e488d1044b79_45_0.png)

> You’d often hear the term **On-premise** being used as an alternative for *On-premises*. The correct term is **On-premises**. The term **premise** is defined by the Chambers Dictionary as *premise noun 1 (also premises) something assumed to be true as a basis for stating something further*. **Premises**, on the other hand, is a term used to denote buildings (among others) and arguably makes a whole lot more sense.

## Selection of the hardware stack

The choice of hardware often depends on the type of solution that is chosen and where the hardware would be located. The proper choice depends on several key metrics such as the type of data (structured, unstructured, or semi-structured), the size of data (gigabytes versus terabytes versus petabytes), and, to an extent, the frequency with which the data will be updated. The optimal choice requires a formal assessment of these variables and will be discussed later on in the book. At a high-level, we can surmise three broad models of hardware architecture:

- **Multinode architecture:** This would typically entail multiple nodes (or servers) that are interconnected and work on the principle of multinode or distributed computing. A classic example of a multinode architecture is Hadoop, where multiple servers maintain bi-directional communication to coordinate a job. Other technologies such as a NoSQL database like Cassandra and search and analytics platform like Elasticsearch also run on the principle of multinode computing architecture. Most of them leverage *commodity servers*, another name for relatively low-end machines by enterprise standards that work in tandem to provide large-scale data mining and analytics capabilities. Multinode architectures are suitable for hosting data that is in the range of terabytes and above.
- **Single-node architecture:** Single-node refers to computation done on a single server. This is relatively uncommon with the advent of multinode computing tools, but still retains a huge advantage over distributed computing architectures. *The Fallacy of Distributed Computing* outlines a set of assertions, or assumptions, related to the implementation of distributed systems such as the reliability of the network, cost of latency, bandwidth, and other considerations. If the dataset is structured, contains primarily textual data, and is in the order of 1–5 TB, in today’s computing environment, it is entirely possible to host such datasets on single-node machines using specific technologies as has been demonstrated in later chapters.
- **Cloud-based architecture:** Over the past few years, numerous cloud-based solutions have appeared in the industry. These solutions have greatly reduced the barrier to entry in big data analytics by providing a platform that makes it incredibly easy to provision hardware resources on demand based on the needs of the task at hand. This materially reduces the significant overhead in procuring, managing, and maintaining physical hardware and hosting them at in-house data center facilities.

Cloud platforms such as Amazon Web Services, Azure from Microsoft, and the Google Compute Environment permit enterprises to provision 10s to 1000s of nodes at costs starting as low as 1 cent per hour per instance.

In the wake of the growing dominance of cloud vendors over traditional brick-and-mortar hosting facilities, several complementary services to manage client cloud environments have come into existence.

Examples include cloud management companies, such as Altiscale that provides big data as a service solutions and IBM Cloud Brokerage that facilitates selection and management of multiple cloud-based solutions.

![](img/8ededb1e0b69e85687f6e488d1044b79_46_0.png)

**The exponential decrease in the cost of hardware:** The cost of hardware has gone down exponentially over the past few years. As a case in point, per Statistic Brain’s research, the cost of hard drive storage in 2013 was approximately 4 cents per GB. Compare that with $7 per GB as recent as 2000 and over $100,000 per GB in the early 80’s. Given the high cost of licensing commercial software, which can often exceed the cost of the hardware, it makes sense to allocate enough budget toward procuring capable hardware solutions. Software needs appropriate hardware to provide optimal performance and providing level importance toward hardware selection is just as important as selecting the appropriate software.
