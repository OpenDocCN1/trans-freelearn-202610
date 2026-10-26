Here, we created a data.frame object named df by combining the vectors num, lower, and upper. The previously independent vectors now comprise columns in df. As we can see, the names of the columns appear on the first line of the printed output of a data.frame object. These are the names of the original vectors, num, lower, and upper. Rows have names as well; these are automatically assigned with the characters 1, 2, 3, and 4 (as it appears to the left of the first column in the printed output).

We can also create the data.frame object in a single step by performing the vector assignments within the data.frame function call itself:

```
> df = data.frame(
+ num = 1:4,
+ lower = c("a","b","c","d"),
+ upper = c("A","B","C","D"))
```

Note that in this example, vector types were different (num is numeric, whereas lower and upper are characters). However, the vectors had the same length; otherwise, an error would have occurred since all columns of data.frame must have the same length.

An important parameter of the data.frame function (and several other functions such as read.csv, which will be introduced in the next section) is stringsAsFactors. The stringsAsFactors parameter controls whether character columns are automatically converted to factors (the default value is TRUE). Within the context of this book, we would usually like to keep the character vectors as characters for greater flexibility (we can always make the conversion to factors ourselves when necessary with the factor function; see the previous chapter for more information). Therefore, a function call preserving character columns will be as follows:

```
> df = data.frame(num, lower, upper,
+ stringsAsFactors = FALSE)
> df
  num lower upper
1   1     a     A
2   2     b     B
3   3     c     C
4   4     d     D
```

The way our table is printed on screen is identical when compared to the previous example. However, using methods, which will be introduced later, we will be able to see that columns 2 and 3 now consist of character vectors rather than factors.