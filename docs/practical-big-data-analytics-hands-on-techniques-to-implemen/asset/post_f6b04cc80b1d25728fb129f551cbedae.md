
Nevertheless, some are provided here to act as references or starting points to help you in forming an initial impression:

- **Data mining:** Data mining refers to the process of extracting information from datasets through running queries or basic summarization methods such as aggregations. Finding the top 10 products by the number of sales from a dataset containing all the sales records of one million products at an online website would be the process of mining: that is, extracting useful information from a dataset. NoSQL databases such as Cassandra, Redis, and MongoDB are prime examples of tools that have strong data mining capabilities.
- **Business intelligence:** Business intelligence refers to tools such as Tableau, Spotfire, QlikView, and others that provide frontend dashboards to enable users to query data using a graphical interface. Dashboard products have gained in prominence in step with the growth of data as users seek to extract information. Easy-to-use interfaces with querying and visualization features that could be used universally by both technical and non-technical users set the groundwork to democratize analytical access to data.
- **Visualization:** Data can be expressed both succinctly and intuitively, using easy-to-understand visual depictions of the results. Visualization has played a critical role in understanding data better, especially in the context of analyzing the nature of the dataset and its distribution prior to more in-depth analytics. Developments in JavaScript, which saw a resurgence after a long period of quiet, such as D3.js and ECharts from Baidu, are some of the prime examples of visualization packages in the open source domain. Most BI tools contain advanced visualization capabilities and, as such, it has become an indispensable asset for any successful analytics product.
- **Statistical analytics:** Statistical analytics refers to tools or platforms that allow end users to run statistical operations on datasets. These tools have traditionally existed for many years, but have gained traction with the advent of big data and the challenges that large volumes of data pose in terms of performing efficient statistical operations. Languages such as R and products such as SAS are prime examples of tools that are common names in the area of computational statistics.
- **Machine learning:** Machine learning, which is often referred to by various names such as predictive analytics, predictive modeling, and others, is in essence the process of applying advanced algorithms that go beyond the realm of traditional statistics. These algorithms inevitably involve running hundreds or thousands of iterations. Such algorithms are not only inherently complex, but also very computationally intensive.

The advancement in technology has been a key driver in the growth of machine learning in analytics, to the point where it has now become a commonly used term across the industry. Innovations such as self-driving cars, traffic data on maps that adjust based on traffic patterns, and digital assistants such as Siri and Cortana are examples of the commercialization of machine learning in physical products.

## Summary

Big data is undoubtedly a vast subject that can seem overly complex at first sight. Practice makes perfect, and so it is with the study of big data--the more you get involved, the more familiar the topics and verbiage gets, and the more comfortable the subject becomes.

A keen study of the various dimensions of the topic of big data analytics will help you develop an intuitive sense of the subject. This book aims to provide a holistic overview of the topic and will cover a broad range of areas such as Hadoop, Spark, NoSQL databases as well as topics that are based on hardware design and cloud infrastructures. In the next chapter, we will introduce the concept of Big Data Mining and discuss about the technical elements as well as the selection criteria for Big Data technologies.

## Big Data Mining for the Masses

Implementing a big data mining platform in an enterprise environment that serves specific business requirements is non-trivial. While it is relatively simple to build a big data platform, the novel nature of the tools present a challenge in terms of adoption by business-facing users used to traditional methods of data mining. This, ultimately, is a measure of how successful the platform becomes within an organization.

This chapter introduces some of the salient characteristics of big data analytics relevant for both practitioners and end users of analytics tools. This will include the following topics:

- What is big data mining?
- Big data mining in the enterprise:
  - Building a use case
  - Stakeholders of the solution
  - Implementation life cycle
- Key technologies in big data mining:
  - Selecting the hardware stack:
    - Single/multinode architecture
    - Cloud-based environments
  - Selecting the software stack:
    - Hadoop, Spark, and NoSQL
    - Cloud-based environments

## What is big data mining?

Big data mining forms the first of two broad categories of big data analytics, the other being Predictive Analytics, which we will cover in later chapters. In simple terms, big data mining refers to the entire life cycle of processing large-scale datasets, from procurement to implementation of the respective tools to analyze them.

The next few chapters will illustrate some of the high-level characteristics of any big data project that is undertaken in an organization.

## Big data mining in the enterprise

Implementing a big data solution in a medium to large size enterprise can be a challenging task due to the extremely dynamic and diverse range of considerations, not the least of which is determining what specific business objectives the solution will address.

## Building the case for a Big Data strategy

Perhaps the most important aspect of big data mining is determining the appropriate use cases and needs that the platform would address. The success of any big data platform depends largely on finding relevant problems in business units that will deliver measurable value for the department or organization. The hardware and software stack for a solution that collects large volumes of sensor or streaming data will be materially different from one that is used to analyze large volumes of internal data.

The following are some suggested steps that, in my experience, have been found to be particularly effective in building and implementing a corporate big data strategy:

- **Who needs big data mining:** Determining which business groups will benefit most significantly from a big data mining solution is the first step in this process. This would typically entail groups that are already working with large datasets, are important to the business, and have a direct revenue impact, and optimizing their processes in terms of data access or time to analyze information would have an impact on the daily work processes. As an example, in a pharmaceutical organization, this could include Commercial Research, Epidemiology, Health Economics, and Outcomes. At a financial services organization, this could include Algorithmic Trading Desks, Quantitative Research, and even Back Office.

- **Determining the use cases:** The departments identified in the preceding step might already have a platform that delivers the needs of the group satisfactorily. Prioritizing among multiple use cases and departments (or a collection of them) requires personal familiarity with the work being done by the respective business groups.

Most organizations follow a hierarchical structure where the interaction among business colleagues is likely to be mainly along **rank lines**. Determining impactful analytics use cases requires a close collaboration between both the practitioner as well as the stakeholder; namely, both the management who has oversight of a department as well as the staff members who perform the hands-on analysis. The business stakeholder can shed light on which aspects of his or her business will benefit the most from more efficient data mining and analytics environment. The practitioners provide insight on the challenges that exist at the hands-on operational level. Incremental improvements that consolidate both the operational as well as the managerial aspects to determine an optimal outcome are bound to deliver faster and better results.

- **Stakeholders' buy-in:** The buy-in of the stakeholders—in other words, a consensus among decision-makers and those who can make independent budget decisions—should be established prior to commencing work on the use case(s). In general, multiple buy-ins should be secured for redundancy such that there is a pool of primary and secondary sources that can provide appropriate support and funding for an extension of any early-win into a broader goal. The buy-in process does not have to be deterministic and this may not be possible in most circumstances. Rather, a general agreement on the value that a certain use case will bring is helpful in establishing a baseline that can be leveraged on the successful execution of the use case.

- **Early-wins and the effort-to-reward ratio:** Once the appropriate use cases have been identified, finding the ones that have an optimal effort-to-reward ratio is critical. A relatively small use case that can be implemented in a short time within a smaller budget to optimize a specific business-critical function helps in showcasing early-wins, thus adding credibility to the big data solution in question. We cannot precisely quantify these intangible properties, but we can hypothesize:

![](img/8ededb1e0b69e85687f6e488d1044b79_41_0.png)
