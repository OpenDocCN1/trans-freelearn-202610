# Reading the table

Once you have completed the preceding steps, the table you have just created will be registered in the databricks system and will remain persistent across sessions, i.e you will not need to reload the data every time you login.

# Running the first cell

Begin by running the first cell (also referred to as a code chunk), which simply will get a count of the number of records by year. You can access the code in this chapter by downloading it from the book's site. Alternatively, you can copy each section of the following code into a new cell and create your own notebook that way.

Since stop frisk has been imported and has already been registered as a table, we can begin to use SQL to read it some of the counts in order to see how large the file is:

```
#embed all SQL within the sql() function
yr <- sql("SELECT year,frisked,count(*) as year_cnt FROM stopfris
display(yr)
```

After a few seconds, the output will appear as a simple formatted table. A simple calculation using the year_cat column shows that for 2015, 68% of those who where stopped, were also frisked:

+   ▶ (5) Spark Jobs

| year | frisked | year_cnt |
|---|---|---|
| 2015 | N | 7306 |
| 2015 | Y | 15257 |