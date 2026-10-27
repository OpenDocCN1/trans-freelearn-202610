
Code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles are shown as follows: "In the current scenario from the `ArtPiece` dataset, we are trying to predict whether a work of art is a good purchase, or not, by taking a few business-relevant variables."

Any command-line input or output is written as follows:

```
> fit <- neuralnet(formula = CurrentAuctionAveragePrice ~ Critic.Ratings + Acq.Cost + CollectorsAveragePrice + Min.Guarantee.Cost, data = train, hidden = 15, err.fct = "sse", linear.output = F)
> fit
Call: neuralnet(formula = CurrentAuctionAveragePrice ~ Critic.Ratings + Acq.Cost + CollectorsAveragePrice + Min.Guarantee.Cost, data = train, hidden = 15, err.fct = "sse", linear.output = F)
1 repetition was calculated.
Error Reached Threshold Steps
1 54179625353167 0.004727494957 23
```

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_14_0.png)

Warnings or important notes appear in a box like this.

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_14_1.png)

Tips and tricks appear like this.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book—what you liked or disliked. Reader feedback is important for us as it helps us develop titles that you will really get the most out of. To send us general feedback, simply e-mail `feedback@packtpub.com`, and mention the book's title in the subject of your message. If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, see our author guide at `www.packtpub.com/authors`.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you could report this to us. By doing so, you can save other readers from frustration and help us improve subsequent versions of this book. If you find any errata, please report them by visiting `http://www.packtpub.com/submit-errata`, selecting your book, clicking on the **Errata Submission Form** link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata will be uploaded to our website or added to any list of existing errata under the Errata section of that title.

To view the previously submitted errata, go to `https://www.packtpub.com/books/content/support` and enter the name of the book in the search field. The required information will appear under the **Errata** section.

## Piracy

Piracy of copyrighted material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works in any form on the Internet, please provide us with the location address or website name immediately so that we can pursue a remedy.

Please contact us at `copyright@packtpub.com` with a link to the suspected pirated material.

We appreciate your help in protecting our authors and our ability to bring you valuable content.

## Questions

If you have a problem with any aspect of this book, you can contact us at `questions@packtpub.com`, and we will do our best to address the problem.

## 1 Data Manipulation Using In-built R Data

The book *R Data Mining Blueprints* focuses mainly on learning methods and steps in performing data mining using the R programming language as a platform. Since R is an open source tool, learning data mining using R is very interesting for learners at all levels. The book is designed in such a way that the user can start from data management techniques, exploratory data analysis, data visualization, and modeling up to creating advanced predictive modeling such as recommendation engines, neural network models, and so on. This chapter gives an overview of the concept of data mining, its various facets with data science, analytics, statistical modeling, and visualization. This chapter gives a glimpse of programming basics using R, how to read and write data, programming notations, and syntax understanding with the help of a real-world case study. This chapter includes R scripts for practice to get hands-on experience of the concepts, terminologies, and underlying reasons for performing certain tasks. The chapter is designed in such a way that any reader with little programming knowledge should be able to execute R commands to perform various data mining tasks.

In this chapter, we will discuss in brief the meaning of data mining and its relations with other domains such as data science, analytics, and statistical modeling; apart from this, we will start the data management topics using R so that you can achieve the following objectives:

- Understanding various data types used in R, including vector and its operations
- Indexing of data frames and factor sequences
- Sorting and merging dataframes and data type conversion
- String manipulation and date object formatting
- Handling missing values and NAs and missing value imputation techniques
- Flow control, looping constructs, and the use of apply functions

### What is data mining?

Data mining can be defined as the process of deciphering meaningful insights from existing databases and analyzing results for consumption by business users. Analyzing data from various sources and summarizing it into meaningful information and insights is that part of statistical knowledge discovery that helps not only business users but also multiple communities such as statistical analysts, consultants, and data scientists. Most of the time, the knowledge discovery process from databases is unexpected and the results can be interpreted in many ways.

The growing number of devices, tablets, smartphones, computers, sensors, and various other digital devices helps in generating and collecting data at a much faster rate than ever before. With the ability of modern-day computers, the increased data can be preprocessed and modeled to answer various questions related to any business decision-making process. Data mining can also be defined as a knowledge-intensive search across discrete databases and information repositories using statistical methodologies, machine learning techniques, visualization, and pattern recognition technologies.

The growth of structured and unstructured data, such as the existence of bar codes in all products in a retail store, attachment of RFID-based tags on all assets in a manufacturing plant, Twitter feeds, Facebook posts, integrated sensors across a city to monitor the changing weather conditions, video analysis, video recommendation based on viewership statistics, and so on creates a conducive ecosystem for various tools, technologies, and methodologies to splurge. Data mining techniques applied to the variety of data discussed previously not only provide meaningful information about the data structure but also recommend possible future actions to be taken by businesses.

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_19_0.png)

Figure 1: Data Mining – a multi-disciplinary subject

The process of data mining involves various steps:

1. Extract the required data from databases and data warehouses.
2. Perform a sanity check on the data to remove redundant characters and irrelevant information.
3. At times, it is important to combine information from various other disjoint databases. Hence, look for common attributes to combine databases.
4. Apply data transformation techniques. Sometimes, it is required to include a few attributes and features in a model.
5. Pattern recognition among the input features, where any of the pattern recognition methods can be applied.
6. Knowledge representation. This includes representation of knowledge mined from the databases in a visual form to various business stakeholders.

![](img/04e03d5ea18ca2e57a47a14c9ae47ace_20_0.png)

Figure 2: A typical data mining process flow

Having discussed the process flow of data mining and the core components, it is also important to look at a few challenges that one may encounter in data mining, such as computational efficiency, unstructured databases and their confluence with structured databases, high-dimensional data visualization, and so on. These issues can be resolved using innovative approaches. In this book, we are going to touch upon a few solutions while performing practical activities on our projects.
