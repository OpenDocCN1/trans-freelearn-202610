
```
install.packages("dplyr")
install.packages("wakefield")
install.packages("sqldf")
install.packages("RSQLite")
```

The convention in this book will be to specify a simple `install.packages("packagename")` instruction to install the necessary packages, followed by a `library("packagename")` to load them into memory. Once the required packages are installed, you can either comment them out, or replace the line with conditional installation code:

```
try(require(dplyr) || install.packages("dplyr"))
```

But this syntax may not work on all R installations and GUIs, so we will keep it simple, and explicitly specify `install.packages()`.

Now we will load the required packages into memory:

```
library(dplyr)
library(wakefield)
library(sqldf)
library(RSQLite)
```

## Generating the data

Next, generate the membership file with 1,000 members and assign it to the member dataframe. The `wakefield` package uses specialized functions to generate typical values for each of the specified variables:

- The `gender` function will generate `M`, or `F` with a 50% chance of any individual row being a male or female
- The `set.seed(1010)` directive guarantees that the results will be the same no matter how many times you run the code
- The `r_sample_replace()` function will generate a unique member ID with a value from 1 to 1000
- The `income`, `children`, `employment`, `level`, `grad`, `year`, `state`, and `zip_code` variables are all randomly generated without supplying any specialized parameters

Open up a new script window, and run the following code:

```
#GENERATE MEMBER
set.seed(1010)
member <- r_data_frame (
  n=1000,
  r_sample_replace(x = 1:1000,replace=FALSE,name="memberid"),
  age,
  gender(x = c("M","F"), prob = c(.5,.5),name="Gender"),
  dob,
  income,
  children,
  employment,
  level,
  grade,
  year,
  state,
  zip_code
)
```

Similarly, generate the purchases file, and assign it to the purchases dataframe. The total amount purchases will come from a normal distribution with a mean purchases amount of 20,000 and a standard deviation of 1,000. The Product variable is a random product name coming from the letters A-Z.

This next code snippet will generate the purchases:

```
#GENERATE PURCHASES
set.seed(1010)

purchases <- r_data_frame(
  n=1000,
  r_sample_replace(x = 1:1000,replace=TRUE,name="memberid2"),
  purch=rpois(lambda=1),
  normal(mean = 20000, sd = 1000, min = NULL, max = NULL, name =
  upper(x = LETTERS, k=3, prob = NULL, name = "Product")
)
purchases$purch <- purchases$purch + 1
str(purchases)
```

After generating the purchases dataframe, we will write it to an external CSV file. The reason we are doing this is to demonstrate how we can filter rows from an external file using read.csv.sql:

```
#WRITE PURCHASES TO FILE
write.csv(purchases, "purchases.csv", quote = FALSE, row.names =
```

Now we will read the `purchases.csv` back into R. But instead of reading the whole file, we will only read in those records where the `TotalAmount` > 20,000:

```
#read it back in
purchases_filtered <- read.csv.sql("purchases.csv", sql = "select
```

You may be asking why we did it this way instead of just reading the whole file and then filtering it later. The answer is efficiency. Let's assume that in reality, the purchases file would be much larger than the 1,000 records it really is, and would contain lots of small purchases. We wouldn't want to take up processing time by reading all of these small transactions, since we are only interested in looking at high-value members. So, you can filter the data while reading the file, and only read in those members who had purchases greater than 20,000.

## Examining the metadata

After reading in any file, it is always a good practice to verify the number of rows, and look at the metadata after a dataframe is created. We can do that easily with the `str()` function. The `str()` function is an incredibly useful function that is packed with a lot of the type of metadata information that we discussed in Chapter 2, The Modeling Process. It is always a good idea to run the `str()` function every time you read, merge, join, or create a new file:

```
str(member)
str(purchases)
str(purchases_filtered)
```

```
> str(member)
Classes 'tbl_df', 'tbl' and 'data.frame':  1000 obs. of  12 variables:
 $ memberid : int  553 192 185 676 909 818 659 971 384 792 ...
 $ Age      : int  30 23 29 20 31 23 26 29 28 28 ...
 $ Gender   : Factor w/ 2 levels "M","F": 2 2 1 2 2 2 1 2 2 1 ...
 $ DOB      : Date, format: "2002-04-30" "2001-12-16" "2002-08-14" "2002-09-08" ...
 $ Income   : num  52826 53330 50949 31842 47418 ...
 $ Children : int  2 1 5 1 3 1 7 0 3 ...
 $ Employment: Factor w/ 5 levels "Full Time","Part Time",..: 4 5 1 1 1 1 1 1 1 4 ...
 $ Level    : int  1 4 4 4 2 3 2 3 1 3 ...
 $ Grade    : num  88.6 78.3 88.7 82.8 83.2 85.8 89.2 91.6 84.4 94.2 ...
 $ Year     : int  2009 1998 2010 2015 2013 1997 2012 1998 2015 2012 ...
 $ State    : Factor w/ 50 levels "Alabama","Alaska",...: 43 5 43 38 43 17 6 4 35 36 ...
 $ Zip      : chr  "82942" "37189" "89850" "49835" ...
```

```
> str(purchases)
Classes 'tbl_df', 'tbl' and 'data.frame':  1000 obs. of  4 variables:
 $ memberid2 : int  553 192 185 678 912 822 663 977 387 799 ...
 $ purch     : num  2 1 2 1 2 1 2 2 2 ...
 $ TotalAmount: num  19457 21222 19469 20331 19003 ...
 $ Product   : chr "B" "C" "C" "C" ...
```

```
> str(purchases_filtered)
'data.frame':   489 obs. of  4 variables:
 $ memberid2 : int  192 678 822 663 387 914 830 974 549 902 ...
 $ purch     : int  1 1 2 2 2 1 1 3 1 ...
 $ TotalAmount: num  21222 20331 21208 21151 21869 ...
 $ Product   : chr "C" "C" "C" "A" ...
```

If you switch over to the console, you can see that both member and purchases have 1,000 rows, and the purchases_filtered dataframe has 489 rows. Looking across the TotalAmount row in str(purchases_filtered) suggests that all of the purchases are all in fact > 20,000.

Always take a look at the number of missing values (NAs), and the levels specified for all of the factors. There are no missing values in the data. Look for any situations in which character variables should be treated as factors, and v.v. For example, the Product variable is listed as a character (chr), when it should probably be treated as a factor. We can always switch the type later.

If you just want to see the number of rows created, you can use the nrow function, instead of the str() function:

```
nrow(member) #1000 members
nrow(purchases_filtered) #489
```

## Merging data using Inner and Outer joins

Now let's merge the membership file with the purchases. In SQL there are two kinds of merge that you can consider when associating two dataframes.

An inner join will consolidate two records based on the matching of a single or multiple key that they both have in common. An outer join will also merge the two tables together by keys, but will also include any rows that are not matched. You can identify an outer join by observing the existence of NAs in any of the matching keys. Inner joins are usually more efficient, but should only be used if you expect to have matching keys of both of the joined files.

The join2 dataframe is an inner join that will contain only those members that had a purchase record. The sqldf() function will match all the observations by member:

```
join2 <- sqldf("select * from 'member' inner join 'purchases_filtered' on member.memberid=purchases_filtered.memberid2")
```

Immediately after the join, perform a str() function on the join2 dataframe:

```
str(join2)
```

The str() function will show that there are 489 observations, indicating that not all members had purchases:

```
> str(join2)
'data.frame':	489 obs. of  16 variables:
 $ memberid : int  192 676 909 971 384 963 541 9 79 272 ...
 $ Age      : int  23 20 31 29 28 20 33 26 26 24 ...
 $ Gender   : Factor w/ 2 levels "M","F": 2 2 2 2 2 1 1 2 1 1 ...
 $ DOB      : Date, format: "2001-12-16" "2002-09-08" "2001-07-24" "2002-03-26" ...
 $ Income   : num  53330 31842 47418 30076 10282 ...
 $ Children : int  1 1 3 7 0 4 0 4 0 3 ...
 $ Employment: Factor w/ 5 levels "Full Time","Part Time",..: 5 1 1 1 1 3 1 5 1 1 ...
 $ Level    : int  4 4 2 3 1 4 1 3 4 ...
 $ Grade    : num  78.3 82.8 83.2 91.6 84.4 83.9 82.7 86.9 85.9 85.1 ...
 $ Year     : int  1998 2015 2013 1998 2015 1996 1999 1999 ...
 $ State    : Factor w/ 50 levels "Alabama","Alaska",..: 5 38 43 4 35 44 38 5 43 9 ...
 $ Zip      : chr "37189" "49835" "45727" "68914" ...
 $ memberid2 : int  192 676 909 971 384 963 541 9 79 272 ...
 $ purch    : int  1 1 2 1 1 2 4 1 2 ...
 $ TotalAmount: num  21222 20239 20118 20439 20874 ...
 $ Product  : chr "C" "C" "A" "C" ...
```

In cases where you do not expect to always have matches from two files, an outer join is usually better. The join3 dataframe will merge members with their purchase records, but will also result in including all members who didn't have any purchase records. This is accomplished by using the outer join clause. After joining the dataframes, issue the str and nrow functions to verify the number of rows. There should be 1,105 rows in the dataframe. This
