

Figure 10-5 – Viewing common assets in a multi-tenant environment

In a similar way to dashboards, other assets (such as datasets or analyses) can be shared using the QuickSight API.

## Summary

Congratulations on completing this chapter, which is the final chapter of this book.

In this chapter, we learned about multitenancy in Amazon QuickSight. We also looked closely at an example architecture that allows multiple tenants to access embedded assets in QuickSight through a web portal. Then, we focused on QuickSight namespaces, and we learned how to configure them. Finally, we looked at two hands-on examples to understand how namespaces prevent users from sharing assets, and how an Admin user can configure common assets for users in different namespaces.

You should now understand the fundamental concepts of multitenancy in Amazon QuickSight, and you should know how to use the AWS CLI to create namespaces and organize your different tenants.

Before you close this book, please ensure that all AWS resources created for this book and its exercises are shut down to prevent charges to your AWS account.

I want to thank you for your time, and I hope this book met your expectations. For any comments and further questions, you can find me on LinkedIn. Feel free to connect with me, I would love to hear your thoughts and feedback. Happy learning!

## Questions

- 1. What are the main characteristics of multi-tenant platforms?
- 2. What are the main use cases for creating multi-tenant BI platforms?
- 3. What are QuickSight namespaces, and how do you configure them?

## Further reading

- Amazon QuickSight User Guide: https://docs.aws.amazon.com/quicksight/latest/user/amazon-quicksight-user.pdf
- Supporting Multitenancy with Isolated Namespaces: https://docs.aws.amazon.com/quicksight/latest/user/namespaces.html

## Packt

Packt.com  
Subscribe to our online digital library for full access to over 7,000 books and videos, as well as industry leading tools to help you plan your personal development and advance your career. For more information, please visit our website.

### Why subscribe?

- Spend less time learning and more time coding with practical eBooks and Videos from over 4,000 industry professionals
- Improve your learning with Skill Plans built especially for you
- Get a free eBook or video every month
- Fully searchable for easy access to vital information
- Copy and paste, print, and bookmark content

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at packt.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at customercare@packtpub.com for more details.

At www.packt.com, you can also read a collection of free technical articles, sign up for a range of free newsletters, and receive exclusive discounts and offers on Packt books and eBooks.

## Other Books You May Enjoy

If you enjoyed this book, you may be interested in these other books by Packt:

![](img/33bad1d211a2d93fd743a1978d403f9a_230_0.png)

### Machine Learning with Amazon SageMaker Cookbook

Joshua Arvin Lat

ISBN: 9781800567030

- Train and deploy NLP, time series forecasting, and computer vision models to solve different business problems Push the limits of customization in SageMaker using custom container images Use AutoML capabilities with SageMaker Autopilot to create high-quality models Work with effective data analysis and preparation techniques Explore solutions for debugging and managing ML experiments and deployments Deal with bias detection and ML explainability requirements using SageMaker Clarify Automate intermediate and complex deployments and workflows using a variety of solutions.

![](img/33bad1d211a2d93fd743a1978d403f9a_231_0.png)

## Amazon Redshift Cookbook

Shruti Worlikar, Thiyagarajan Arumugam, Harshida Patel

ISBN: 9781800569683

- Use Amazon Redshift to build petabyte-scale data warehouses that are agile at scale Integrate your data warehousing solution with a data lake using purpose-built features and services on AWS Build end-to-end analytical solutions from data sourcing to consumption with the help of useful recipes Leverage Redshift’s comprehensive security capabilities to meet the most demanding business requirements Focus on architectural insights and rationale when using analytical recipes Discover best practices for working with big data to operate a fully managed solution.

## Packt is searching for authors like you

If you're interested in becoming an author for Packt, please visit **authors.packtpub.com** and apply today. We have worked with thousands of developers and tech professionals, just like you, to help them share their insight with the global tech community. You can make a general application, apply for a specific hot topic that we are recruiting an author for, or submit your own idea.

## Share Your Thoughts

Now you've finished _Actionable Insights with Amazon QuickSight_, we'd love to hear your thoughts! If you purchased the book from Amazon, please click here to go straight to the Amazon review page for this book and share your feedback or leave a review on the site that you purchased it from.

Your review is important to us and the tech community and will help us make sure we're delivering excellent quality content.

## Index

### A

- actions
  - filter actions 106-108
  - navigation actions 108, 109
  - URL actions 109-111
  - working with 106
- ad-hoc analytics 7
- advanced dataset operations
  - calculated fields, adding 59, 60
  - datasets, filtering 61
  - datasets, joining 61
  - working with 59
- Amazon API Gateway 136
- Amazon Athena 8, 140
- Amazon Athena dataset
  - about 182
  - configuring, with Amazon Lake Formation 184, 185
  - permissions, configuring without Amazon Lake Formation 182-184
- Amazon Cognito 138
- Amazon EC2 instances 140
- Amazon EKS 136
- Amazon EventBridge 166
- Amazon Lake Formation
  - about 182
  - Amazon Athena dataset, permissions configuring 182-184
  - integrating with 182
  - used, for configuring Amazon Athena dataset 184, 185
- Amazon Lambda 136
- Amazon Macie 7
- Amazon QuickSight
  - about 6, 28, 140
  - architecture 32
  - domain, whitelisting 136, 137
  - end-to-end BI process 31
  - networking concepts 32
  - setting up 36-40
  - user authorization options 34, 35
- Amazon QuickSight, core concepts
  - analysis 28
  - dashboards 29
  - datasets 28
  - insights 29
  - visuals 29
- Amazon QuickSight, data sources
  - adding 42, 43
  - configuring 44
  - Redshift cluster access, enabling 44, 45
  - Redshift data source, setting up 46-49
  - S3 data source, setting up 49
- Amazon QuickSight, editions
  - about 34
  - enterprise edition 34
  - standard edition 34
  - standard edition, versus enterprise edition 35
- Amazon QuickSight permissions
- Amazon Lake Formation
  - integrating with 182
  - managing 176
  - setting up 179-181
  - user groups, using 177-179
- Amazon QuickSight usage
  - managing 186
- QuickSight threshold-based alerts, working with 193
- Amazon QuickSight, user types
  - about 29
  - admin 30
  - author 30
  - reader 30
- Amazon Redshift cluster 155
- Amazon Resource Number (arn) 176
- Amazon S3 dataset
  - application, testing 171, 172
  - creating 167-169
- QuickSight ingestion lambda
  - function, configuring 169-171
  - trigger, configuring 171
- Amazon S3 data source
  - about 166
  - ingestion, automating 167
- Amazon Simple Storage Service (S3) 6, 136, 140
- Amazon SPICE data source 167
- Amazon STS 139
  - anomalies exploring 128-130
- API Gateway 140
- application
  - chart color settings, modifying 88
  - color of visual, modifying 87, 88
  - conditional formatting, applying to visual 88-90
  - interface, customizing 84
  - themes, applying 85-87
  - visuals, formatting 87
- application programming interface (API) 150
- authentication layer 135-138
- autodiscover feature 43
- AutoGraph 29
- autonarrative features 118
- AWS
  - modern data architecture, exploring on 10
  - modern data architecture on 11
- AWS analytics ecosystem
  - ad-hoc analytics 7
  - business intelligence (BI) 5
  - data lake governance 6, 7
  - data lake storage 6, 7
  - data warehousing 6
  - discovering 4
  - Extract, Transform, Load (ETL) 8, 9
- AWS CLI
  - used, for accessing QuickSight API 150-153
- AWS documentation reference link 153
- AWS Glue Catalog 7
- AWS Glue Data Catalog 182
- AWS IoT Analytics 43

