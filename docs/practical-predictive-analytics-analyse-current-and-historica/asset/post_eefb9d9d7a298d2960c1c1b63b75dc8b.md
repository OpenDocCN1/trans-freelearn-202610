
- 2 Dealing with my lender or servicer 0.6967735
- 3 Getting a loan 0.4779246

## References

- AMERICAN STATISTICAL ASSOCIATION RELEASES STATEMENT ON STATISTICAL SIGNIFICANCE AND P-VALUES. (2016, March 7). Retrieved from ASA news: http://www.amstat.org/asa/files/pdfs/P-ValueStatement.pdf

- Anscombe's quartet. Retrieved from Wikipedia: https://en.wikipedia.org/wiki/Anscombe%27s_quartet

- Coefficient of determination. Retrieved from Wikipedia: https://en.wikipedia.org/wiki/Coefficient_of_determination

- Fader, P. S. (2001, May-June). Forecasting Repeat Sales at CDNOW. Interfaces, 31 (May-June), Part 2 of 2, S94-S107.

- FAQ: WHAT ARE PSEUDO R-SQUAREDS? Retrieved from UCLA Institute for Digital Research and Education: http://stats.idre.ucla.edu/other/mult-pkg/faq/general/faq-what-are-pseudo-r-squareds/

## Summary

In this chapter, we added three more algorithms to our arsenal, and these 3, along with regression form the core basic algorithms that can cover a lot of ground in terms of the typical problems a predictive analyst will face. We saw that a good knowledge of decision tree methodologies allows you to start developing models quickly, they are easily interpretable, and are the basis for more advanced techniques such as random forests. We then went on to clustering. Clustering allows you to begin to grasp the concepts of similarity and dissimilarity, and we introduced distance measures. We then ended with a basic introduction to support vector machines, which were demonstrated in the context of text mining.

In the next chapter, we will begin to look at some examples of creating models that predict how long a customer will stay with a company, or for predicting how long it will be until a patient develops a certain medical condition.

## Chapter 6. Using Survival Analysis to Predict and Analyze Customer Churn

> "In an Infinite Universe anything can happen," said Ford, "Even survival. Strange but true."

- Douglas Adams The Restaurant at the End of the Universe (1980)

## What is survival analysis?

Survival analysis covers a broad range of topics. Here is the list of topics that we will cover in this chapter:

- Survival analysis
- Time-based variables and regression
- R survival objects
- Customer attrition or churn
- Survival curves
- Cox regression
- Plotting methods
- Variable selection
- Model concordance

Often, predictive analytic problems deal with various situations concerning the tracking of important events along a customer's journey, and predicting when these events will occur. Survival analysis is a form of analysis that is based upon the concept of time to event. The time to event is simply the number of units of time that have elapsed until something happens. The event can be just about anything; a car crash, a stock market crash, or a devastating phenomenon.

Survival analysis originated in the studying of patients who developed terminal diseases, such as cancer, hence the term survival. However, conceptually, it can even be applied to marketing applications in which you are following the occurrence of an event over a customer's lifetime. In this case, the time to event can mean a customer response or purchase.

In our example, we will use a customer churn example. Customer churn is a term that you will hear often in the context of how a company retains its customers. Churn is very important, since it often costs more to acquire new customers than it would to offer some discounts or promotions to existing customers, in order to keep them happy so they don't leave.

There are many different statistical modeling techniques you can choose for churn analysis, including regression, decision trees, random forests, naive bayes, and neural networks.

Survival analysis is a good technique to use with customer churn problems since it is able to deal specifically with two aspects of marketing data, which other techniques have problems with. These involve the concepts of time-dependent data and censoring.

### Time-dependent data

In many analytic techniques, all data is treated as static at the time of analysis, and these techniques are not well equipped to deal with data that changes over time. Any event that measures a change in a variable over time, such as age, or changes in attitudes via surveys can be treated as time-dependent variables, and are handled well by survival techniques. This works well within a marketing concept since the treatment of different customer segments can change, and we need a mechanism to measure the effect of interventions such as promotions and advertising that can change customer behavior over time.

### Censoring

In general, censoring is a term that is used to describe data that is only partially known. The reason it is considered partially known is that all data is constrained between the starting and ending periods of an observational study. This can be information that occurs before a study has begun or after a study has ended. If all the information is contained within the study period itself, the data is not censored. However, this rarely happens.

Censored data can be left or right censored.

### Left censoring

In a marketing context, studies usually begin with customers already in place, and without any knowledge of how they were acquired.

Additionally, not all customers start at the same time. When a customer has started prior to the beginning of a study or analysis, some customer attributes can be referred to as left censored. If you start an analysis that includes all of your customers, you will not have knowledge of some of the events that preceded the start of the study. This knowledge may include very important information, such as customer dissatisfaction with a prior customer policy that was implemented way before the start of the study, but there is not much we can do about it if we don’t have the information. In a survival analysis, everyone starts off on an even footing.

### Right censoring

Studies also have an ending period, and customers who may leave a day after the study ends would still be considered active. This is another example of data that wouldn’t be recorded. Therefore, a customer is considered right censored if a study ends before the customer terminates. However, it also includes some other special situations, such as when a customer is still active at the end of the study, or is lost due to events not related to the study’s variables.

For example, a customer who has been active only for 10 days would still be expected to be active if the study happens to be ending a short time after that; or if a customer did not respond to follow-up questionnaires due to a change of address, they could be thrown out of the study. These are all naturally occurring events.

In the following diagram, the black vertical lines indicate the beginning and end dates of a hypothetical churn study. The first dot in each row indicates the point at which the customer was acquired, and the last dot indicates the point in time that the customer left. Only the data available within the begin and end date period is available for analysis. The diagram shows vertical lines in places where a customer could be considered as left or right censored:

- As indicated in the top bar, we see that a customer was acquired after the study began and left before the study ended. Therefore, we have complete information about the customer with regard to tenure, and the customer is considered not censored.
- The middle bar shows a customer that was acquired after the study began; however, the customer is still active at the end of the study, and the dot slightly past the right vertical line shows that the customer left shortly after the end of the study. However, this churn activity will not be reflected in the data, so this is an example of right censored data.
- The bottom bar shows a customer who was acquired prior to the start of the study, with an unknown start date. This is an example of a left censored observation:

![](img/672c0cf8ba3c000a6529b117ed96945e_283_0.png)
