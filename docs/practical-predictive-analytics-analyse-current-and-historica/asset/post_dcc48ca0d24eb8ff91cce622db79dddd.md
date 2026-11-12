
any association meaningless or nonactionable.

So the first line of attack for this is to try to understand how the data was collected. It may be biased towards certain age groups, genders, technology users, and so on. There may be ways to fix this using a technique known as **oversampling** in which you weigh certain under-represented groups in a way which more realistically represents their frequency.

## Comparing a sample to the population

To illustrate some of the benefits of sampling, and to see how you can often get close to the same results with a sample as with a larger population, copy the following code and run it within an R script. This script will generate a 15,000,000 row population and then extract a 100-row random sample. Then we will compare the results:

```
large.df <- data.frame(
gender = c(rep(c("Male", "Female", "Female"), each = 5000000)),
purchases = c(0:9, 0:5, 0:7)
)
#take a small sample
y <- large.df[sample(nrow(large.df), 100), ]
mean(large.df$purchases)
mean(y$purchases)
#Render 2 plots side-by-side by setting the plot frame to 1 by 2
par(mfrow=c(1,2))
barplot(table(y$gender) /sum(table(y$gender)) )
barplot(table(large.df$gender) /sum(table(large.df$gender)))
#Return the plot window to a 1 by 1 plot frame
par(mfrow = c(1, 1))
```

Switch over to the console and note that the sample mean is close to the population mean:

```
> mean(large.df$purchases)
[1] 3.666667
> mean(y$purchases)
[1] 3.64
```

Now, switch over to the plot area and note that the distribution of gender is also similar when comparing the sample (left plot), and the population (right plot):

![](img/672c0cf8ba3c000a6529b117ed96945e_61_0.png)

I encourage you to take some large datasets on your own, practice taking some random samples and seeing how close you get to population results. Possibly that will enable you to work faster and in a more productive manner.

## An analytics methodology outline with specific steps

This section will look at each of the analytics methodology steps individually. I will use CRISP-DM as the template, because it covers model deployment, and we have already mentioned the benefits of sampling (which is the first step in SEMMA).

## Step 1 business understanding

Many predictive modelers assume that the actual modeling phase is where the most insightful model development takes place. However, much of the groundwork and insight can be discovered early on, and a good understanding of business objectives can avoid pitfalls later on.

## Communicating business goals and the feedback loop

I must admit, business people and technical people can be better at communicating with each other. How business goals are communicated can run the gamut. It can be anything from a business partner stating, "Tell me how sales need to be increased" or "Tell me something I don't know."

So, it really starts with understanding what the specific business objectives are. During this step, you will also get involved in breaking down the babble and buzzwords into specifics so that communication can begin using data as a common language. As exchanges begin between the domain experts and data scientists, it will become clear that business people often do know what they are looking for in terms of outcomes, but do not understand the analytic modeling process well enough to verbalize their goals. This is often the case in reverse for technical people who do not understand the nuances of the business.

The understanding phase is characterized by people asking many questions, and establishing an analytic dialogue between all parties. So, even with a more precise question like, "Who are our best customers?", the dialog can go as follows:

- 1. What do you mean by best?
- 2. Do you mean best now or best compared to what it was a year ago?
- 3. What metric are you using to define best? Do you mean best in terms of revenue or in terms of frequency of purchase?
- 4. How will we measure success? What are the key metrics? What are the possible outcomes?

The process of business understanding also involves an understanding of where, how, and when you obtain your data.

## Internal data

Your own internal data is the first and best place to start. Many projects start with examining data from business operational systems, such as sales systems. You can start by analyzing transaction data, such as looking at all orders for a customer and analyzing the number of units sold, total sales price, and so on. However, transaction data by itself does not tell you anything about the customer. You need to also enrich your basic data by augmenting it with other attributes which can be obtained by matching common elements of your database transactions (such as customer ID, product keys) with valuable data which exists in other related databases. For example, a rich source of data is demographic and attitudinal data which may be stored in company customer surveys.

In addition to customer surveys, there may be other internal repositories that you can use to derive insight. Web logs may reveal more information about customer clickstream buying patterns. Data can be captured from customer call center data, HR and other administrative systems.

Finding other data sources often involves a lot of digging, since often internal data is siloed in large organizations.

## External data

Data can also be augmented by integrating a company's internal data combined with data obtained from a variety of sources, including government data, social media feeds, and data purchased from vendors. Often demographic, behavioral, and risk data is purchased separately, and then merged within the data warehouse.

Beware of the daunting task of data integration. A key problem encountered in this task is being able to associate data from a variety of disparate sources with each other. This is less of a problem when dealing with internal data, but when dealing with external data you may have to use alternative methods available to perform matching, such as using fuzzy match (similarity) algorithms or performing entity extraction methods (which, for example, can extract a customer name). These are just two ways which can help you with associating data with external sources.

## Tools of the trade

Defining the objectives can be tough at the beginning, and it is not advisable to start using advanced analytic tools at the business understanding stage. The best tools to use at this stage are somewhat low-tech and include important concepts such as process understanding, data dictionaries, and data linkage, and Structured Query Language (SQL).

## Process understanding

An understanding of how data is generated in your domain as well as how it has been generated in the past is important. This means understanding how data has been transformed from its original raw state to how it is consumed by the ultimate user of the data. Understanding the imperfections of the current process will enable you to get a sense of which data can be consistently relied upon. Additionally, try to gather a history of what data changes and methodologies have been attempted in the past. This is important since the different ways that data used might have been quite different, and the goals might have been different. Knowing what has succeeded and what has failed in the past will prevent you from reinventing the wheel, keep you from replicating the very same mistakes, and help you build upon what has been attempted before.

### Data lineage

Data lineage tools are helpful for documentation, and for following the changes of the nuanced meaning of data as it progresses from raw data form, to data storage, and then to the final definition. The general term for the type of data that lineage tools deal with is known as **metadata** (data about data).

### Data dictionaries

Data dictionaries can be valuable as a source for understanding the types of variables under analysis and how they are measured. Here are some useful metadata items to keep in a data dictionary:

- **Name of the variable**: Consistency in naming conventions helps in understanding and readability. Some analysts like to use CamelCase, others like to use punctuation for an object, as in `Variable.data.frame`, and others will insist on only having lowercase letters.
- **Measurement data**: This answers questions such as, "Is the data numeric or categorical?", "How many levels are contained in each category?", and "What is the length of each variable?"
- **Sources of the data**: This covers, "Where did the data originally came from?"
- **Transformations**: This answers, "How was the data manipulated from its original form to what it is now?"
- **Data quality items**: Attaching frequency distributions and summary statistics to each variable will be helpful, along with any comments regarding any questionable data quality.

It is important to keep a data dictionary up to date, because variables or their values can change meaning over time. For example, a marketing offer with the value of A100 could mean we give a 15% discount to the customer now, but upon closer inspection, the same A100 code happened to be used five years ago in a different marketing application where it meant 10% discount to the customer.

Here is an example of a simple data dictionary:
