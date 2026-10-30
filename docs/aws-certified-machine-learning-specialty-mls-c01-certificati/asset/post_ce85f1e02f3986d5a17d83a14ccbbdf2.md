
## 8
# Evaluating and Optimizing Models

Introducing model evaluation 254

Exploring other regression metrics 261

Evaluating classification models 255

Model optimization 261

Grid search 262

Extracting metrics from a confusion matrix 256

Summary 264

Summarizing precision and recall 259

Questions 265

Evaluating regression models 259

## 9
# Amazon SageMaker Modeling

Technical requirements 272

Choosing the right instance type for an inference job 290

Creating notebooks in Amazon SageMaker 272

Securing SageMaker notebooks 291

What is Amazon SageMaker? 272

Creating alternative pipelines with Lambda Functions 292

Getting hands-on with Amazon SageMaker notebook instances 276

Creating and configuring a Lambda Function 294

Getting hands-on with Amazon SageMaker's training and inference instances 279

Completing your configurations and deploying a Lambda Function 296

Model tuning 282

Working with Step Functions 300

Tracking your training jobs and selecting the best model 287

Summary 302

Choosing instance types in Amazon SageMaker 288

Questions 303

Choosing the right instance type for a training job 290

Answers 305

Why subscribe? 307

# Other Books You May Enjoy

# Index

# Preface

The AWS Machine Learning Specialty certification exam tests your competency to perform **machine learning (ML)** on AWS infrastructure. This book covers the entire exam syllabus in depth using practical examples to help you with your real-world machine learning projects on AWS.

Starting with an introduction to machine learning on AWS, you'll learn the fundamentals of machine learning and explore important AWS services for **artificial intelligence (AI)**. You'll then see how to prepare data for machine learning and discover different techniques for data manipulation and transformation for different types of variables. The book also covers the handling of missing data and outliers and takes you through various machine learning tasks such as classification, regression, clustering, forecasting, anomaly detection, text mining, and image processing, along with their specific ML algorithms, that you should know to pass the exam. Finally, you'll explore model evaluation, optimization, and deployment and get to grips with deploying models in a production environment and monitoring them.

By the end of the book, you'll have gained knowledge of all the key fields of machine learning and the solutions that AWS has released for each of them, along with the tools, methods, and techniques commonly used in each domain of AWS machine learning.

# Who this book is for

This book is for professionals and students who want to take and pass the AWS Machine Learning Specialty exam or gain a deeper knowledge of machine learning with a special focus on AWS. Familiarity with the basics of machine learning and AWS services is necessary.

# What this book covers

*Chapter 1, Machine Learning Fundamentals, covers some machine learning definitions, different types of modeling approaches, and all the steps necessary to build a machine learning product, known as the modeling pipeline.*

*Chapter 2, AWS Application Services for AI/ML*, covers details of the various AI/ML applications offered by AWS, which you should know to pass the exam.

*Chapter 3, Data Preparation and Transformation*, deals with categorical and numerical features, applying different techniques to transform your data, such as one-hot encoding, binary encoding, ordinal encoding, binning, and text transformations. You will also learn how to handle missing values and outliers on your data, two important topics to build good machine learning models.

*Chapter 4, Understanding and Visualizing Data*, teaches you how to select the most appropriate data visualization technique according to different variable types and business needs. You will also learn about available AWS services for visualizing data.

*Chapter 5, AWS Services for Data Storing*, teaches you about AWS services used to store data for machine learning. You will learn about the many different S3 storage classes and when to use each of them. You will also learn how to handle data encryption and how to secure your data at rest and in transit. Finally, we will present other types of data store services, still worth knowing for the exam.

*Chapter 6, AWS Services for Processing*, teaches you about AWS services used to process data for machine learning. You will learn how to deal with batch and real-time processing, how to directly query data on Amazon S3, and how to create big data applications on EMR.

*Chapter 7, Applying Machine Learning Algorithms*, covers different types of machine learning tasks, such as classification, regression, clustering, forecasting, anomaly detection, text mining, and image processing. Each of these tasks has specific algorithms that you should know about to pass the exam. You will also learn how ensemble models work and how to deal with the curse of dimensionality.

*Chapter 8, Evaluating and Optimizing Models*, teaches you how to select model metrics to evaluate model results. You will also learn how to optimize your model by tuning its hyperparameters.

*Chapter 9, Amazon SageMaker Modeling*, teaches you how to spin up notebooks to work with exploratory data analysis and how to train your models on Amazon SageMaker. You will learn where and how your training data should be stored in order to be accessible through SageMaker and the different data formats that you can use.

# To get the most out of this book

You will need a system with a good internet connection and an AWS account.

If you are using the digital version of this book, we advise you to type the code yourself or access the code via the GitHub repository (link available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

# Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/AWS-Certified-Machine-Learning-Specialty-MLS-C01-Certification-Guide. In case there's an update to the code, it will be updated on the existing GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

# Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/9781800569003_ColorImages.pdf.

# Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: To check each of the versions and the latest one of them we use aws s3api list-object-versions --bucket version-demo-mlpractice to which S3 provides the list-object-versions API, as shown here.

A block of code is set as follows:

```
"Versions": [
{
"ETag":
"\"b6690f56ca22c410a2782512d24cdc97\"",
"Size": 10,
"StorageClass": "STANDARD",
```

```
"Key": "version-doc.txt",
"VersionId":
"70wbLG6BMBEQhCXmwsriDgQoXafFmgGi",
"IsLatest": true,
"LastModified": "2020-11-07T15:57:05+00:00",
"Owner": {
"DisplayName": "baba",
"ID": "XXXXXXXXXXXX"
}
} ]
```

When we wish to draw your attention to a particular part of a code block, the relevant lines or items are set in bold:

```
[default]
exten => s,1,Dial(Zap/1|30)
exten => s,2,Voicemail(u100)
exten => s,102,Voicemail(b100)
exten => i,1,Voicemail(s0)
```

Any command-line input or output is written as follows:

```
$ aws s3 ls s3://version-demo-mlpractice/
$ echo "Version-2">version-doc.txt
```

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For example, words in menus or dialog boxes appear in the text like this. Here is an example: "Select **System info** from the **Administration** panel."

Tips or important notes
Appear like this.

# Get in touch

Feedback from our readers is always welcome.

**General feedback**: If you have questions about any aspect of this book, mention the book title in the subject of your message and email us at customercare@packtpub.com.
