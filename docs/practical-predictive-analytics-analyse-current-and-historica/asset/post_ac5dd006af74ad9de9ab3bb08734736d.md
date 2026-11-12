
x
str(x)
binned <- x
binned
levels(binned$cats) <- substring(levels(binned$cats), 1, 4)
binned
> cats <- as.factor(c('2016-1','2016-2','2016-3'))
> sales <- c(10,20,30)
> x <- cbind.data.frame(cats,sales)
> x

cats sales
1 2016-1 10
2 2016-2 20
3 2016-3 30
> str(x)
'data.frame':   3 obs. of  2 variables:
$ cats : Factor w/ 3 levels "2016-1","2016-2",...:  1 2 3
$ sales: num  10 20 30
> binned <- x
> binned

cats sales
1 2016-1 10
2 2016-2 20
3 2016-3 30
> levels(binned$cats) <- substring(levels(binned$cats), 1, 4)
> binned

cats sales
1 2016 10
2 2016 20
3 2016 30

Now all the dates have been binned to the proper year, based upon an embedded text string:

```
aggregate(binned$sales, by=list(binned$cats), sum )
```

The results are written to the console:

```
> aggregate(binned$sales, by=list(binned$cats), sum)
  Group.1 x
1    2016 60
```

## Missing values

Missing values denote the absence of a value for a variable. Since data can never be collected in a perfect manner, many missing values can appear due to human oversight, or can be introduced via any systematic process that touches a data element. It can be due to a survey respondent not completing a question, or, as we have seen, it can be created from joining a membership file with a transaction file. In this case, if a member did not have a purchase in a particular year, it might end up as NA or missing.

The first course of action for handling missing values is to understand why they are occurring. In the course of plotting missing values, you not only want to produce counts of missing values, but you want to determine which sub-segments may be responsible for the missing values.

To research this, attempt to break out your initial analysis by time periods and other attributes using some of the bivariate analysis techniques that have been mentioned. This will help you to identify where missing values may be tucked away.

### Setting up the missing values test dataset

We will start off by using two groups of generated data. One group is for males, who have a 3% probability of not responding to an age question in a survey, and the other group is for females, who have a 5% probability of not responding to an age question:

```r
library(wakefield)
library(dplyr)

#generate some data for Males with a 5% missing value for age

set.seed(10)
f.df <- r_data_frame(
 n = 1000,
 age,
gender(x = c("M", "F"), prob = c(0,1),name="Gender"),
education
) %>%
r_na(col=1,prob=.05)

#str(f.df)
summary(f.df)
set.seed(20)
#generate some data for Females with a 3% missing value for age

m.df <- r_data_frame(
  n = 1000,
  age,
  gender(x = c("M", "F"), prob = c(1,0),name="Gender"),
  education
) %>%
r_na(col=1,prob=.03)
summary(m.df)

all.df=rbind.data.frame (m.df,f.df)
```

Note that we have generated missing values by piping the output of the r_data_frame() function into the r_na() function, which will generate the specified percentage of missing values. After the script completes, switch over to the console to verify that NAs were generated for the age variable. There are 80 of them:

| | Age | Gender | Education |
|---|---|---|---|
| Min. | :20.00 | M:1000 | Regular High School Diploma :522 |
| 1st Qu. | :23.00 | F:1000 | Bachelor's Degree :323 |
| Median | :28.00 | | Some College, 1 or More Years, No Degree:295 |
| Mean | :27.54 | | 9th Grade to 12th Grade, No Diploma :169 |
| 3rd Qu. | :32.00 | | Master's Degree :150 |
| Max. | :35.00 | | Associate's Degree :140 |
| NA's | :80 | | (Other) :401 |

## The various types of missing data

There are actually three different categories of missing value that you should become familiar with. Understanding the type of missing value will help you determine how to handle them.

## Missing Completely at Random (MCAR)

Some percentage of missing values will always occur naturally. When this happens, the missing data is known as Missing Completely at Random (MCAR). An example of this is if 2% of a survey was not recorded due to a glitch in a survey response system. If this were the case, one could assume that it was not related to any other variable in the data. MCAR variables are also not correlated with its own data values, that is it would also not be related to any other of the non-missing values that appear in the data.

### Testing for MCAR

When you suspect that your data is MCAR, there are various statistical tests can help you determine if MCAR may be occurring. One important test is the Little test, which we will now run on the test missing value dataset.

First, install the BaylorEdPsych package, which contains the LittleMCAR test:

```
try (require(BaylorEdPsych) || install.packages("BaylorEdPsych", de
library(BaylorEdPsych)
```

Now, run the LittleMCAR test:

```
test_mcar<-LittleMCAR(all.df)
```

Print the missing values found by the test. 4% of the data was found to be missing. This makes perfect sense, since 5% of the 1,000 males and 3% of the 1,000 females were generated with NAs.

```
print(test_mcar$amount.missing)
print(test_mcar$p.value)
```

```
> print(test_mcar$amount.missing)
     Age Gender Education
Number Missing   80.00   0        0
Percent Missing  0.04    0        0
```

Print the value of the test statistic. High values of p imply the data is MCAR, while low values imply there is some pattern. At the .05 significance level, it passes the MCAR test (but barely). We also know that we generated our own patterns of NA via the simulation, so that certainly contributed to the p-value of .07:

```
> print(test_mcar$p.value)
[1] 0.06782777
```

Alternatively, you can use Chi-square and regressions tests to determine if the missingness of a variable is related to another variable. If you find that there is no statistical significance between the missing variable and any other variable, you can consider the variable MCAR. These tests can be valuable, although the methods are not fool proof. There are still assumptions that need to be made about the process that generated them. As always, the best course is to investigate the process that generated the values to see how they were generated.

## Missing at Random (MAR)

Missing at random (MAR) implies that the missingness of a value is related to another variable in the analysis. It is somewhat of a misnomer, since there is nothing random about it.

So, another question to ask when dealing with missing values is "Is the fact that there are missing values in this variable related to the dynamics of any other variable?" Again, using our survey analogy as an example, missing values for, "What is your Expected Salary?" might occur for certain demographic groups who did not answer the question. If you found a statistically significant number of missing values at one level of a factor, but not as many within another level, that would suggest that the data was missing at random. A variable that is MAR can be found by looking at some of the same techniques used previously, that is, Chi-square, regression, and Little test, and if differences are found you can consider the variable MAR. For our test missing dataset, we might consider this MAR since there is a difference in the number of missing values for males and females.

## Not Missing at Random (NMAR)

A variable that is not MCAR and not MAR can be considered **Not Missing at Random (NMAR)**. That basically means that the missing value is related to the values of the missing value itself, or is related to a variable that's not in the model. NMAR values often show up in survey research and clinical research where subjects are measured over time. For example, a survey respondent who reported severe depression at the beginning of a study may drop out at the end of the study, so if they scored high on a depression scale, that would be related to the fact they have missing scores at a later date, since they might be more inclined to drop out.

NMAR is the most difficult situation to detect, since a variable may seem to be MAR or MCAR and really be NMAR. Missing value analysis is not an exact method, and again, that is where understanding the domain, and the underlying methods of generating data, is so important.

## Correcting for missing values

Although it is always important to understand the source of your missing values, how you ultimately handle them depends upon the technique that you use to analyse your data sets. For example, classification methods such as decision trees and random forests know how to deal with missing values, since they can treat them as a separate class, and you can safely leave them in the model. However, if a variable has a large amount of missing values, say > 20%, you might want to look at imputation techniques, or try to find a better variable that measures the same thing.

### Listwise deletion

For MCAR, eliminating rows that have missing values is acceptable. In this example (from [https://en.wikipedia.org/wiki/Listwise_deletion](https://en.wikipedia.org/wiki/Listwise_deletion)), observations 3, 4, and 8 will be removed from the data prior to analysis:
