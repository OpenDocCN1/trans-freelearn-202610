beginning of the data set as NA. LAST is FALSE. By keeping NAs separate from the order function, data sanity can be maintained.

The merge function helps in combining two data frames. To combine two data frames, at least one column name should be identical. The two data frames can also be joined by using a function called column bind. To display the difference between the column bind and merge functions, we have taken audit.csv dataset. There are two small datasets, A and B, prepared out of the audit dataset:

```
> A<-audit[,c(1,2,3,7,9)]
> names(A)
[1] "ID"      "Age"     "Employment" "Income"   "Deductions"
> B<-audit[,c(1,3,4,5,6)]
> names(B)
[1] "ID"        "Employment" "Education"  "Marital"    "Occupation"
```

Two columns, ID and Employment, are common in both datasets A and B, which can be used as a primary key for merging two data frames. Using the merge command, the common columns are taken once in the result dataset from the merge function. The merged data frame contains all the rows that have complete entries in both the data frames:

```
> head(merge(A,B),3)
          ID Employment Age Income Deductions Education Marital Occupation
1 1004641 Private    38 81838.0          0 College Unmarried Service
2 1010229 Private    35 72099.0          0 Associate Absent Transport
3 1024587 Private    32 154676.7         0 HSgrad Divorced Clerical
```

The merge function allows four different ways of combining data: natural join, full outer join, left outer join, and right outer join. Apart from these joins, two data frames can also be merged using any specific column or multiple columns. Natural join helps to keep only rows that match from the data frames, which can be specified by the all=F argument:

```
> head(merge(A,B, all=F),3)
          ID Employment Age Income Deductions Education Marital Occupation
1 1044221 Private    60 7568.23          0 College Married Executive
2 1047095 Private    74 33144.40         0 HSgrad Married Service
3 1047698 Private    43 43391.17         0 Bachelor Married Executive
```