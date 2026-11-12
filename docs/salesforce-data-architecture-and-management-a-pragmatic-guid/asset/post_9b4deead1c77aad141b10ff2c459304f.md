
- Putting it all together 204
- Problem 205
- The solution approach 205
- Summary 207
- Questions 208
- Further reading 209

## 6 Managing Performance

- Salesforce Platform performance 212
- Importance of performance monitoring 212
- Reasons for performance-related issues 215
- Performance tools 220
- URL suffixes 221
- Speedtest 221
- Salesforce Optimizer 223
- Salesforce Shield's event monitoring 223
- Salesforce Page Optimizer 226
- Salesforce Lightning Inspector 226
- Salesforce reports 226
- Query Plan tool 227
- Improving performance 234
- Conducting performance testing in Salesforce 254
- Approaching performance testing in Salesforce 256
- Conducting a successful performance test 259
- Monitoring performance 261
- What to monitor? 261
- Summary 262
- Questions 263
- Further reading 264

## Section 3: Large Data Volumes (LDVs) and Data Migrations

## 7 Working with Large Volumes of Data

- Revisiting databases 270
- Relational databases 271
- Non-relational databases 272
- Large Data Volumes (LDVs) 276
- Knowing the implications of LDVs 277
- Multitenancy and search architecture 278
- Considerations for integrating or migrating LDVs 279
- Preventing LDV scenarios 284
- Optimizing LDV operations 287
- Salesforce Connect and external objects 296
- Ways to connect 297
- Introducing big objects 299
- Benefits of big objects 300
- Considerations for big objects 301
- Use cases for big objects 303

- Summary 305
- Questions 306
- Further reading 307

## 8 Best Practices for General Data Migration

- Assessing data 310
- Components of a data migration assessment 311
- The Preparation phase 313
- Best practices 314
- The Execution phase 316
- Best practices 316
- Tools for loading data into Salesforce 319
- Data Import Wizard (DIW) 320
- Salesforce Data loader 320
- Understanding APIs 321
- The SOAP API 322
- The Bulk API 323
- The REST API 328
- The Streaming API 328
- Summary 330
- Questions 331
- Further reading 332

## Assessments

- Chapter 1 – Data Architect Roles and Responsibilities 333
- Answers 333
- Chapter 2 – Understanding Salesforce Objects and Data Modeling 333
- Answers 333
- Chapter 3 – Understanding Data Management 333
- Answers 333
- Chapter 4 – Making Sense of Master Data Management 334
- Answers 334
- Chapter 5 – Implementing Data Governance 334
- Answers 334
- Chapter 6 – Managing Performance 334
- Answers 334
- Chapter 7 – Working with Large Volumes of Data 334
- Answers 334
- Chapter 8 – Best Practices for General Data Migration 335
- Answers 335

## Other Books You May Enjoy

## Index

## Preface

Data is becoming more and more valuable to organizations as they realize its true potential and how it can help them to achieve their business objectives. The reason for this is that data can be turned into actionable intelligence that organizations can leverage to keep their internal and external customers happy while maintaining their competitive edge.

The key here is actionable intelligence derived from data otherwise it takes up storage space and is an expense to the organization. The Salesforce Platform facilitates turning unactionable data into actionable intelligence, but for this to happen on an on going basis, the data architecture must be properly designed and allow scalability. Data modeling blunders include creating roll-up summary fields on an account record that have the year hard coded in them to show account revenue and require the roll-up summary filter to be updated every year. Blunders also include worse situations where a lookup relationship may have been used for an application, but a parent-child relationship was created, leading to a tightly coupled relationship. The sole reason for choosing this type of relationship would be due to it providing the roll-up summary field capability.

In this book, we will start with the very basics of understanding what data architects are expected to do and how you can be a successful Salesforce data architect. You will then learn about data modeling and data management. Once we have the basics covered, we will delve into master data management, data governance, and how we can ensure performance for our applications.

Data keeps on growing at a fast pace and knowing how to design solutions effectively involving large volumes of data is a crucial skill set. Therefore, we will extensively cover Large Data Volumes (LDVs) and what we can do as architects to effectively manage them while keeping scalability and Salesforce governor limits under consideration.

I have included questions at the end of each chapter to ensure you have clearly understood the concepts discussed in the chapter. Moreover, there are extensive examples and diagrams throughout the chapters to ensure that a firm understanding of the concepts is developed.

When all is said and done and you have reached the end of the book, you will have gained a solid understanding of data architectural skills and best practices that you can apply immediately within the context of Salesforce. The content covered in this book is also very relevant to the Data Architecture and Management Designer exam and the data architecture domain that is tested in the Salesforce Certified Technical Architect (CTA) exam (https://trailhead.salesforce.com/credentials/dataarchitectureandmanagementdesigner).

## Who this book is for

This book is for both aspiring and experienced architects, Salesforce admins, and developers who want to learn more about the core architecture of the Salesforce platform in the context of data management and architecture. Whether you are just starting off in the Salesforce ecosystem as an admin or a developer, or whether you are an experienced architect with several years of experience under your belt, this book has something for everyone to benefit from.

To get the most out of the book, you will have had experience with data migration and integrations in large, complex technology landscapes consisting of disparate systems. You will also have had to deal with data with privacy regulations and topics such as master data management to further manage data in your technology knowledge landscape. Salesforce professionals that have large orgs with millions of records who are concerned about performance will also benefit from the book as scalability and performance have been at the forefront when writing.

Although the book covers a multitude of topics that overlap different Salesforce certification exams, Salesforce professionals specifically preparing for the Data Architecture and Management Designer exam or the Salesforce CTA review board will find the book helpful as supporting material for these exam preparation.

## What is this book covers

Chapter 1, Data Architect-Roles and Responsibilities, describes the role of a data architect and the core skills and experience that are required for it. It will also go in to detail on what soft skills are required to be successful in the role. You will also get to have a look at a day in the life of a data architect.

Chapter 2, Understanding Salesforce Objects and Data Modeling, will take you through the unique architecture of the Salesforce platform and how it is optimized for read access rather than write operations traditionally seen in relational databases. Data modeling concepts, how they get applied in the context of Salesforce, what de-normalization is, and why it is important to spend the time designing your data model properly will be discussed. At the end of the chapter, Salesforce objects and how they are created, different types of fields on them, and their use cases will be covered.

Chapter 3, Understanding Data Management, will explain data management, what it is, and why it's important. The different aspects of managing data, including the data lifecycle, will also be discussed. With Salesforce's discontinuance of data recovery services, data backup and archiving have come to the forefront, so we will discuss that in detail as well. At the end, some tools that are available to manage data effectively will be reviewed.

Chapter 4, Making Sense of Master Data Management, will discuss the key attributes of master data, what the Golden Record is and why it is so important for organizations. We will look at how to align your MDM and CRM strategy with a discussion on Salesforce's Customer 360 and its key components. MDM is a platform-agnostic concept that can be used within the context of non-Salesforce landscapes as well. The chapter is concluded with a brief discussion of the Common Information Model (CIM).

Chapter 5, Implementing Data Governance, covers the importance of enterprise data governance, the relationship between data governance and data management, and how to assess the current state of data governance. Two major privacy protection laws, the GDPR and CCPA, will also be covered in detail. To conclude and firm up understanding of the content, a sample case study will describe a hypothetical scenario and the solution approach to solve it.

Chapter 6, Managing Performance, will explore foundational aspects of performance on the Force.com platform, how to use the Query Plan tool to determine performance-impacting queries, and query costs when using indexes versus full table scans. The chapter also covers the various tools that can be used to monitor the platform for performance and auditing changes. Multiple code blocks will be used to drive the point home of how performance can be determined and optimized. Performance testing is critical especially when dealing with large volumes of data, so an extensive discussion around aspects of performance testing will be covered, followed by a discussion on monitoring the performance of the Salesforce org.

Chapter 7, Working with Large Volumes of Data, will introduce you to the concept of relational and non-relational databases. LDVs, which are becoming more and more relevant in the Salesforce ecosystem as orgs generate or consume lots of data, will be discussed extensively, from identifying LDV scenarios to managing LDV orgs and integrating data into these org types.
