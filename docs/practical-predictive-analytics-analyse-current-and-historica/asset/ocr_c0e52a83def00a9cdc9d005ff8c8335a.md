# # Subsetting the columns

For this exercise, we will be using a restricted set of columns from the CSV file. We can either select the specific columns from the dataframe just read in (if we just read in the whole file), or reread the csv file using the `colClasses` parameter to only read the columns that are required. Often, this method is preferable when you are reading a large file, and will instruct `read.csv` to only retain the first three and the last two columns, and ignore the columns `priemp` through `govmilitary`.

After rereading in the file, with a subset of the columns, we print a few records from the beginning and end of the file. We can do this using a combination of the `rbind()`, `head()`, and `tail()` functions. This will give us all of the columns we will be using for this chapter, except for some columns, which we will derive in the next section:

```
x <- read.csv("hihist2bedit.csv", colClasses = c(NA,NA, NA, NA, r
rbind(head(x), tail(x))
> 	 Year Year.1 Total.People Total Not.Covered
> 1 			 2012 2012 311116.15 263165.47 47950.6840
> 2 			 2011 2011 308827.25 260213.79 48613.4625
> 3 	 2010 (10) 2010 306553.20 256602.70 49950.5004
> 4 			 2009 2009 304279.92 255295.10 48984.8204
> 5 			 2008 2008 301482.82 256702.42 44780.4031
> 6 			 2007 2007 299105.71 255017.52 44088.1840
> 331 	 2004 (4) 2004 20062.67 19804.54 258.1313
> 332 			 2003 2003 19862.49 19615.92 246.5703
> 333 			 2002 2002 19705.99 19484.01 221.9879
> 334 			 2001 2001 19533.99 19354.19 179.8033
> 335 	 2000 (3) 2000 19450.52 19250.63 199.8871
> 336 	 1999 (2) 1999 19378.56 19189.17 189.3922
>
> 1 			 ALL AGES
> 2 			 ALL AGES
> 3 			 ALL AGES
> 4 			 ALL AGES
> 5 			 ALL AGES
> 6
```