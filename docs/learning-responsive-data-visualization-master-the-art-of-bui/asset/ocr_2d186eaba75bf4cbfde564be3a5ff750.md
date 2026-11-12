There are some questions that come into one's mind if he/she wants to plot the preceding data. How should one aggregate the data? Do we want to sum all visitors per minute, per hour, or per day, or do we just want to sum the visitors by browser or by country of origin? Once we've parsed the data into a readable format, we can easily do all of the preceding things (you will learn this in the last section of the chapter). However, we first need to parse the string into a format that JavaScript can use.

# Splitting strings using Regular Expressions

The first step in processing a string is always splitting it into smaller chunks of words. If the log entries have been created by a program, they usually have some kind of structure but very often a very strange one.

If we take a closer look at the entries in the log file, we can see that single log entries are separated by a newline character called \n. Well, we actually don't see the character, but we can see the line breaks only at the end of the complete log entry.

We also observe that the - and " characters are some sort of separators between the logical parts of the entry. To split the string properly, we always have to find these separators. Now, we can use Regular Expressions and the string.split(expr) function to split the string into an array of smaller chunks. In the following figure, we can see this operation visualized:

![](bbox=[0.345, 0.48, 0.69, 0.76])

Let's see this in action and split the log file into an array of log entries:

```
var lines = data.split(/\n/g);
```