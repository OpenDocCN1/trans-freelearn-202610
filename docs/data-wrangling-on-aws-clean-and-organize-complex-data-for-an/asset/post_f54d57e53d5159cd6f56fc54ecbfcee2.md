
An **Identity and Access Management (IAM)** role with the appropriate policy and permissions attached. There is an Amazon SageMaker Full Access AWS managed policy that you can use as is or as a starting point to create your custom policy.

### Studio domain

You will start by creating and onboarding a SageMaker domain using the AWS console. A SageMaker domain includes an **Amazon Elastic File System (Amazon EFS)** volume, a list of authorized studio users, and a variety of security, application, policy, and **Amazon Virtual Private Cloud (Amazon VPC)** configurations. When you are creating a domain, you can choose who you want to onboard users on the domain. You have two options: use AWS IAM Identity Center (successor to **AWS Single Sign-On**, or AWS SSO) or AWS IAM for authentication methods. Choose the IAM Identity Center option if you are integrating SageMaker Studio with an existing SSO or creating a new SSO setup; else, choose the IAM authentication option. It is important to remember that you cannot change this after the domain has been created unless you delete and recreate the domain. When you use IAM authentication, you can choose either the **Quick setup** or the **Standard setup** option for onboarding a domain. For the scope of this book, we will consider IAM authentication with **Quick setup**.

### Studio onboarding steps

Let's look at the steps for studio onboarding:

1. Log in to your AWS account and navigate to the SageMaker console.
2. From the **Control panel** option on the left, choose **Studio**, as illustrated here:

![](img/e28602b44ec813a47df57c96b204412c_161_0.png)

3. Click on the **Launch SageMaker Studio** button. If this is your first time, you will be taken to the **Setup SageMaker Domain** page with options for **Quick setup** and **Standard setup**:

![](img/e28602b44ec813a47df57c96b204412c_162_0.png)

Figure 4.7 – Domain setup options

**Quick setup** is selected by default. Keep that unchanged.

4. Next, you need to provide a username for the default admin user and choose a default execution role for your domain, as illustrated in the following screenshot. If you do not already have a SageMaker execution role in your account, you can create one using the built-in policy mentioned in the *Prerequisites* section:

![](img/e28602b44ec813a47df57c96b204412c_162_1.png)

Figure 4.8 – SageMaker Studio execution role

SageMaker Canvas is not in scope for this book. You can either leave the option enabled or disable it here before you proceed.

5. Once you click on **Submit**, the domain creation process is started. Behind the scenes, SageMaker will create a Studio domain for you, provision the AWS services required (such as EFS), and create a default user for the domain. This will take a few minutes, and the domain status will show as **Pending** till it completes:

![](img/e28602b44ec813a47df57c96b204412c_163_0.png)

Figure 4.9 – Checking domain creation status

6. After domain creation is complete, the status will change to **Ready**, and you can log in to SageMaker Studio using the default user. To log in, choose **Studio** from the **Launch app** button in the console, as illustrated here:

![](img/e28602b44ec813a47df57c96b204412c_163_1.png)

Figure 4.10 – Launching SageMaker Studio

7. The first time logging in to SageMaker Studio takes a few minutes as it needs to provision a default application. Subsequent logins are quicker. Once the application creation is complete and the login is successful, the SageMaker Studio IDE landing page should open:

![](img/e28602b44ec813a47df57c96b204412c_164_0.png)

That’s it—from here on, you are ready to follow along with all the Data Wrangler examples in the book.

### Summary

In this chapter, we covered an introduction to data processing on AWS, specifically focusing on ML and data science. We looked at how data processing for ML is unique and why it is such a critical and significant component of the overall ML workflow. We went through some of the challenges when dealing with large and distributed datasets and data sources and how to work with these at scale. We discussed the importance of having a reliable and repeatable data processing workflow for ML. We then covered some of the key capabilities that are needed in tooling and the frameworks used for data processing for ML, which include the ability to detect bias present in real-world data, the ability to detect and fix data imbalances, the ability to perform quick and error-free transformations and run pre-processing reports and visualizations at scale, as well as the ability to ingest data at scale.

As enterprises move from experimentation and research to production, the focus switches to operations. Here, we covered data processing for ML in the context of operations and, specifically, MLOps. We discussed why it is important for the tools and frameworks to integrate seamlessly with pipelines at the enterprise. Next, we had an introduction to SageMaker Data Wrangler, a SageMaker service that is purpose-built for data processing for ML. We did a detailed walkthrough of the key capabilities of SageMaker Data Wrangler and how they help solve some of the specific data processing challenges that we covered earlier. A data processing tool is only as important as the data source it can support, and we looked at the supported data sources for Data Wrangler, as well as the integration points with other AWS and third-party services. Once the data processing tasks and transformations are done, we discussed how the processed data can be exported. We looked at the various formats in which data can be exported, as well as the various export destinations supported by Data Wrangler. In addition to the processed data, we also need the processing steps and transformations to be consistent and repeatable; here, we looked at how the transformations can be exported in the form of data flows.

SageMaker Data Wrangler is a component of SageMaker Studio. One of the prerequisites for using Data Wrangler is to set up SageMaker Studio. We wrapped up the chapter by going over how to set up a SageMaker Studio domain in your AWS account and how to onboard users and accounts. Data processing is one of the most challenging and time-consuming areas of ML. If done correctly, it can lead to a direct performance gain for your ML models. SageMaker Data Wrangler provides capabilities that enable data scientists and ML practitioners to perform data processing at scale while integrating with the organization’s overall ML workflow. The **Quick setup** option uses default settings and allows internet access and shareable notebooks.

# Part 3: AWS Data Management and Analysis

In this section, we’ll explore essential AWS services for effective data management and analysis. Chapter 5 introduces Amazon S3, revealing its potential as a robust storage and organization solution for your data. Chapter 6 dives into AWS Glue, demonstrating how this powerful ETL service automates data preparation and transformation workflows. Chapter 7 unveils Amazon Athena, a serverless query service enabling valuable insights through SQL queries directly on Amazon S3 data. Finally, Chapter 8 explores Amazon QuickSight, a fast BI service for creating interactive dashboards and visualizations to gain valuable insights from your data.

This part has the following chapters:

- Chapter 5, Working with Amazon S3
- Chapter 6, Working with AWS Glue
- Chapter 7, Working with Athena
- Chapter 8, Working with QuickSight

# 5 Working with Amazon S3

In previous chapters, we repeatedly discussed the concepts of big data and data lakes and how organizations are using them to store and extract valuable insights from their data through various data wrangling processes, as outlined in *Chapter 1*, using **Amazon Web Services** (AWS) services such as AWS Glue DataBrew, the AWS SDK for Pandas, and SageMaker Data Wrangler. This chapter will delve deeper into the specifics of big data and data lakes.

Specifically, we will be covering the following topics:

- The definition and concept of big data
- The characteristics of big data
- The concept and definition of a data lake
- Best practices for building a data lake on **Amazon Simple Storage Service** (Amazon S3)
- The layout and organization of data on Amazon S3

We will begin by exploring the definition and characteristics of big data.

## What is big data?

Big data refers to extremely large datasets that are too complex and diverse to be processed and analyzed using traditional data management and analytics tools. Big data often comes from multiple sources, such as sensors, social media, and e-commerce platforms, and it may include structured, semi-structured, and unstructured data.

The volume, velocity, and variety of big data present significant challenges for data management and analysis. Traditional data storage and processing systems are not designed to handle such large and complex datasets, and they may not be able to provide the performance, scalability, and flexibility required for big data applications.
