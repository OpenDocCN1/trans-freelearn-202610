saved as SAS data sets, enabling you to access the modeling methods of Chapters 3 through 6 as needed. In fact, it is even possible to generate SAS code to do the scoring.

This chapter is not intended to serve as a comprehensive guide to SAS Enterprise Miner. The intricacies of model building and selection, actions central to data mining, are presented only to the extent that their discussion helps you understand the resulting data curves. Various intermediate steps that we will use here, such as missing data imputation, may not be appropriate for some other data mining exercises. For a solid background on SAS Enterprise Miner, consult Introduction to Data Mining Using SAS Enterprise Miner by Patricia Cerrito (2006).

The audience for this chapter is SAS Enterprise Miner users, especially those who have not used ROC curves with the software. It is assumed here that you are already familiar with ROC curves at the Chapter 3 level, so a cursory reading of Chapter 3 may be useful at this point.

# 9.2 Home Equity Loan Example

The data for the example in this chapter are from the sample data library of SAS Enterprise Miner, Sampsio.Hmeq. The library Sampsio is internally defined when SAS Enterprise Miner is loaded and includes this data set. It contains nearly 6,000 records and 13 variables, one of which is the outcome variable, or target, in the language of data mining. The data set depicts the home equity loan records of a financial services company. Approximately 20% of the past home equity credit lines have resulted in default. In an attempt to reduce the default rate, or perhaps generate applicant-tailored interest rates, the company wants to build a model that predicts the risk of default on a home equity loan. The target variable is called BAD, with a value of 1 indicating a default and a 0 indicating a serviced loan. The 12 predictor variables, along with a brief definition, are as follows:

+   - CLAGE: age (months) of oldest credit line
+   - CLNO: number of credit lines
+   - DEBTINC: debt-to-income ratio
+   - DELINQ: number of delinquent credit lines
+   - DEROG: number of major derogatory reports
+   - JOB: occupation of the debtor
+   - LOAN: amount of the loan
+   - MORTDUE: amount due on existing mortgage
+   - NINQ: number of recent credit inquiries
+   - REASON: reason for loan (debt consolidation or home improvement)
+   - VALUE: value of the property
+   - YOJ: years at present job

The goal is to find a predictive model for the BAD variable using the 12 other variables in the data set. Although we will go through the steps required to build a predictive model, the focus is on constructing ROC curves using SAS Enterprise Miner’s built-in functionality, as well as exploring ways to export the predicted values to a SAS data set so that the methods and macros discussed in earlier chapters can be used to perform custom analyses.