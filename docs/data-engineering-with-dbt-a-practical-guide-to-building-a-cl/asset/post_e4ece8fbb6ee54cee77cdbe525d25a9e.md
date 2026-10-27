
## 13 Moving Beyond the Basics

| Section | Page |
|---|---:|
| Technical requirements | 427 |
| Building for modularity | 428 |
| Modularity in the storage layer | 429 |
| Modularity in the refined layer | 431 |
| Modularity in the delivery layer | 434 |
| Managing identity | 436 |
| Identity and semantics – defining your concepts | 436 |
| Different types of keys | 437 |
| Main uses of keys | 440 |
| Master Data management | 440 |
| Data for Master Data management | 441 |
| A light MDM approach with DBT | 442 |
| Saving history at scale | 445 |
| Understanding the save_history macro | 447 |
| Understanding the current_from_history macro | 455 |
| Summary | 458 |

## 14 Enhancing Software Quality

| Section | Page |
|---|---:|
| Technical requirements | 459 |
| Refactoring and evolving models | 460 |
| Dealing with technical debt | 460 |
| Implementing real-world code and business rules | 462 |
| Replacing snapshots with HIST tables | 463 |
| Renaming the REF_ABC_BANK_SECURITY_INFO model | 466 |
| Handling orphans in facts | 468 |
| Calculating closed positions | 473 |
| Calculating transactions | 482 |
| Publishing dependable datasets | 484 |
| Managing data marts like APIs | 485 |
| What shape should you use for your data mart? | 485 |
| Self-completing dimensions | 487 |
| History in reports – that is, slowly changing dimensions type two | 492 |
| Summary | 493 |
| Further reading | 493 |

## 15 Patterns for Frequent Use Cases

| Section | Page |
|---|---:|
| Technical requirements | 495 |
| Ingestion patterns | 495 |
| Basic setup for ingestion | 498 |
| Loading data from files | 501 |
| External tables | 505 |
| Landing tables | 507 |

*Chapter 2, Setting Up Your DBT Cloud Development Environment,* gets started with DBT by creating your GitHub and DBT accounts. You will learn why version control is important and what the data engineering workflow is when working with DBT.

You will also understand the difference between the open source DBT Core and the commercial DBT Cloud. Finally, you will experiment with the default project and set up your environment for running basic SQL with DBT on Snowflake and understand the key functions of DBT: ref and source.

*Chapter 3, Data Modeling for Data Engineering,* shows why and how you describe data, and how to travel through different abstraction levels, from business processes to the storage of the data that supports them: conceptual, logical, and physical data models.

You will understand entities, relationships, attributes, **entity-relationship (E-R)** diagrams, modeling use cases and modeling patterns, Data Vault, dimensional models, wide tables, and business reporting.

*Chapter 4, Analytics Engineering as the New Core of Data Engineering,* showcases the full data life cycle and the different roles and responsibilities of people that work on data.

You will understand the modern data stack, the role of DBT, and analytic engineering. You will learn how to adopt software engineering practices to build data platforms (or DataOps), and about working as a team, not as a silo.

*Chapter 5, Transforming Data with DBT,* shows us how to develop an example application in dbt and learn all the steps to create, deploy, run, test, and document a data application with dbt.

*Chapter 6, Writing Maintainable Code,* continues the example that we started in the previous chapter, and we will guide you to configure dbt and write some basic but functionally complete code to build the three layers of our reference architecture: staging/storage, refined data, and delivery with data marts.

*Chapter 7, Working with Dimensional Data,* shows you how to incorporate dimensional data in our data models and utilize it for fact-checking and a multitude of purposes. We will explore how to create data models, edit the data for our reference architecture, and incorporate the dimensional data in data marts. We will also recap everything we learned in the previous chapters with an example.

*Chapter 8, Delivering Consistency in Your Code,* shows you how to add consistency to your transformations. You will learn how to go beyond basic SQL and bring the power of scripting into your code, write your first macros, and learn how to use external libraries in your projects.

*Chapter 9, Delivering Reliability in Your Data,* shows you how to ensure the reliability of your code by adding tests that verify your expectations and check the results of your transformations.

*Chapter 10, Agile Development,* teaches you how to develop with agility by mixing philosophy and practical hints, discussing how to keep the backlog agile through the phases of your projects, and a deep dive into building data marts.

*Chapter 11, Collaboration,* touches on a few practices that help developers work as a team and the support that dbt provides toward this.

*Chapter 12, Deployment, Execution, and Documentation Automation*, helps you learn how to automate the operation of your data platform, by setting up environments and jobs that automate the release and execution of your code following your deployment design.

*Chapter 13, Moving beyond Basics*, helps you learn how to manage the identity of your entities so that you can apply master data management to combine data from different systems. At the same time, you will review the best practices to apply modularity in your pipelines to simplify their evolution and maintenance. You will also discover macros to implement patterns.

*Chapter 14, Enhancing Software Quality*, helps you discover and apply more advanced patterns that provide high-quality results in real-life projects, and you will experiment with how to evolve your code with confidence through refactoring.

*Chapter 15, Patterns for Frequent Use Cases*, presents you with a small library of patterns that are frequently used for ingesting data from external files and storing this ingested data in what we call history tables. You will also get the insights and the code to ingest data in Snowflake.

## To get the most out of this book

| Software/hardware covered in the book | Operating system requirements |
|---|---|
| dbt | Windows, macOS, or Linux |

If you are using the digital version of this book, we advise you to type the code yourself or access the code from the book’s GitHub repository (a link is available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Data-engineering-with-dbt. If there’s an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: “Create the new database using the executor role. We named it PORTFOLIO_TRACKING.”

## Share Your Thoughts

Once you’ve read *Data Engineering with dbt*, we’d love to hear your thoughts! Please `click here` to go `straight to the Amazon review page` for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we’re delivering excellent quality content.

## Download a free PDF copy of this book

Thanks for purchasing this book!

Do you like to read on the go but are unable to carry your print books everywhere?
Is your eBook purchase not compatible with the device of your choice?

Don’t worry, now with every Packt book you get a DRM-free PDF version of that book at no cost.

Read anywhere, any place, on any device. Search, copy, and paste code from your favorite technical books directly into your application.

The perks don’t stop there, you can get exclusive access to discounts, newsletters, and great free content in your inbox daily

Follow these simple steps to get the benefits:

- 1. Scan the QR code or visit the link below

  ![](img/8868d272a75eee8b407a8e0f3434d10f_20_0.png)

  ```
  https://packt.link/free-ebook/9781803246284
  ```

- 2. Submit your proof of purchase
- 3. That’s it! We’ll send your free PDF and other benefits to your email directly
