
## Working with Tables

Using the `[` operator, we can obtain a subset of a `data.frame` object, which will include the intersection of any number of rows and columns. This works the same way as vectors subsetting with one difference, that is, a `data.frame` is a two-dimensional object while a vector is a one-dimensional object. Therefore, we need to provide two indices rather than just one.

Remember that when subsetting with the `[` operator in `data.frame`, the first index refers to rows and the second index refers to columns. This arrangement is going to appear in other contexts as well (for example, in matrices and rasters).

The two vectors of indices, used in order to create a subset of a `data.frame` object, can include any combination of the following:

- A numeric vector, in which case the numeric vector refers to the indices of rows/columns to retain in the subset
- A character vector, in which case the character vector indicates the names of rows/columns to retain
- A logical vector, in which case the logical vector indicates whether to retain each row/column of `data.frame`

We have been extensively using methods 1 and 3 in vectors (see the previous chapter), so extending the methods to the two-dimensional case should be intuitive. In fact, method 2 can also be used with vectors since vectors can have element names the same way that a `data.frame` object has row names and column names (but we are not going to use that here).

For example, the following expression gives us the element populating the second row and the third column of `df`:

```r
> df[2, 3]
[1] "B"
```

Leaving an empty space instead of the row's or column's index indicates we are interested in all the elements of the respective dimension (all rows or all columns). For example, the following expressions return all elements of the second row and the third column of `df`:

```r
> df[2, ]
```

|   | num | lower | upper |
|---|-----|-------|-------|
| 2 | 2   | b     | B     |

```r
> df[, 3]
[1] "A" "B" "C" "D"
```

By default, a subset of a `data.frame` object is converted into a simpler class if values from a single column are involved. For example, the `df[2, 3]` and `df[, 3]` expressions returned (character) vectors. The `df[2, ]` expression returned a `data.frame` object since three columns are involved. In fact, the second row of `df` contains both numeric and character values, while we already know that a vector can only contain values of the same type. If we wish, we can suppress the `data.frame` simplification by using the `drop` parameter, indicating `FALSE` (instead of the default value, `TRUE`), and then the subset will remain a `data.frame` object no matter what:

```r
> df[ , 3, drop = FALSE]
```

|   | upper |
|---|-------|
| 1 | A     |
| 2 | B     |
| 3 | C     |
| 4 | D     |

Compare the output to the one from the previous example. Using `drop=FALSE` in the previous expression, we got a `data.frame` object (with four rows and one column) instead of a vector.

The other two methods of subsetting a `data.frame` object are using logical and character vectors as indices. Let's take a look at the following example:

```r
> df[df$lower %in% c("a","d"), c("lower","upper")]
```

|   | lower | upper |
|---|-------|-------|
| 1 | a     | A     |
| 4 | d     | D     |

In this expression, if we put it in plain language, we are requesting to get the subset of `df` with the rows being where the values of the `lower` column are either `"a"` or `"d"`, and the columns are both `lower` and `upper`.

One very helpful function to use with `data.frame` objects is `complete.cases`. This function returns a logical vector, the same length as the number of rows in the `data.frame` object, indicating whether each row (case) is complete (has no `NA` values in it). When a given row is complete, the respective element in the logical vector will be `TRUE`; when a row is incomplete, the value will be `FALSE`. Then, the resulting logical vector can be used to remove the incomplete rows from a table as follows (note that the output is not printed here to save space):

```r
> dat[complete.cases(dat), ]
```

Note that in this expression, we use the vector returned by `complete.cases` as a logical vector indicating the selection of rows in `dat` to be retained.

### Calculating new data fields

As previously shown, we can assign new values to a column of a table (or to a subset of a column) using the `$` operator. If the column name we assign does not exist in the table, a new column will be created to accommodate the data. Let's take a look at the following examples:

```r
> df
```

|   | num | lower | upper |
|---|-----|-------|-------|
| 1 | 1   | a     | A     |
| 2 | 2   | b     | B     |
| 3 | 3   | c     | C     |
| 4 | 4   | d     | D     |

```r
> df$word[df$num == 2] = "Two"
> df
```

|   | num | lower | upper | word  |
|---|-----|-------|-------|-------|
| 1 | 1   | a     | A     | `<NA>` |
| 2 | 2   | b     | B     | Two   |
| 3 | 3   | c     | C     | `<NA>` |
| 4 | 4   | d     | D     | `<NA>` |

Here, we made an assignment of the character value `"Two"` to a subset of the `word` column (which did not previously exist in `df`) corresponding to the rows where the value of column `num` is equal to 2. As a result, a new column has been created, which contains the assigned value (and `NA` for all the other elements). Note that `<NA>` is simply the character representation of `NA` (the `word` column is a character vector).

As another example with our climatic data, we are going to create two new columns, holding the year and month of each measurement. For this purpose, we will first convert the `date` column to a `Date` object. Then, we will extract the years and months from the data in this column as follows (see the previous chapter for details):

```r
> dat$date = as.Date(as.character(dat$date), format = "%Y%m%d")
> dat$month = as.numeric(format(dat$date, "%m"))
> dat$year = as.numeric(format(dat$date, "%Y"))
> head(dat)
```

|   | station          | station_name | elevation | latitude | longitude | date       | tpcp | mmxt | mmnt | month | year |
|---|------------------|--------------|-----------|----------|-----------|------------|------|------|------|-------|------|
| 1 | GHCND:SP000060010 | IZANA SP     | 2371      | 28.3089  | -16.4992  | 1984-01-01 | 51.4 | 5.6  | -0.5 | 1     | 1984 |
| 2 | GHCND:SP000060010 | IZANA SP     | 2371      | 28.3089  | -16.4992  | 1984-02-01 | 0.0  | 9.8  | 2.8  | 2     | 1984 |
| 3 | GHCND:SP000060010 | IZANA SP     | 2371      | 28.3089  | -16.4992  | 1984-03-01 | 68.7 | 7.2  | 0.2  | 3     | 1984 |
| 4 | GHCND:SP000060010 | IZANA SP     | 2371      | 28.3089  | -16.4992  | 1984-04-01 | 13.6 | 12.2 | 3.5  | 4     | 1984 |
| 5 | GHCND:SP000060010 | IZANA SP     | 2371      | 28.3089  | -16.4992  | 1984-05-01 | 21.4 | 11.8 | 3.0  | 5     | 1984 |
| 6 | GHCND:SP000060010 | IZANA SP     | 2371      | 28.3089  | -16.4992  | 1984-06-01 | 6.5  | 19.6 | 10.5 | 6     | 1984 |

Using the first expression, we converted the `dat$date` vector to a `Date` object (and assigned it back to `dat$date`). In the second and third expressions, we extracted the month and year components, as numeric vectors, out of `dat$date` and assigned them to the new columns, `dat$month` and `dat$year`, respectively.

### Writing a data.frame object to a CSV file

A `data.frame` object can be written to a CSV file with the `write.csv` function. The two first (and most important) parameters for this function indicate the name of the `data.frame` object, which we would like to save, and the path to the new file (including the new filename). These parameters have no defaults, so we need to specify them. For example, the following expression writes the `data.frame` object `df` to the `df.csv` file in the `C:\Data` directory:

```r
> write.csv(df, "C:\\Data\\df.csv")
```

The newly created file when opened in Excel looks like the following screenshot:

![](img/ebfd6294770b225fdeceda9a7b8b560e_88_0.png)

Note that row names (the numbers 1 to 4 in column A) and column names have been added; this behavior can be disabled when required.

## Controlling code execution

So far, all of the code sections we have written were executed once in the same order as they were sent to the command line. However, one of the most important themes in programming is the flow control—operations that are used to control the sequences of our code execution. For example, we may want to induce the execution of a certain code section only if a condition is met (these are called conditional statements), or we may wish to execute a code section several times, over and over again (these are called loops). In this section, you will learn about three flow control commands: two to construct conditional statements and one to construct loops.

### Conditioning execution with conditional statements

The purpose of conditional statements is to condition the execution of a given code section. For example, the second expression in the following code section is a conditional statement using the `if` operator:

```r
x = 3
if(x > 2) {print("x is large!")}
[1] "x is large!"
```

A conditional statement is composed of the following elements:

- The conditional statement operator (`if`)
- The condition in parentheses (for example, `(x>2)`)
- Code section opening brackets (`{`)
- The code section to execute when the condition is met (for example, `print("x is large!")`)
- Code section closing brackets (`}`)
- Optionally, the `else` operator (`else`)
- Optionally, code section opening brackets (`{`)
- Optionally, the code to execute when the condition is not met
- Optionally, code section closing brackets (`}`)
