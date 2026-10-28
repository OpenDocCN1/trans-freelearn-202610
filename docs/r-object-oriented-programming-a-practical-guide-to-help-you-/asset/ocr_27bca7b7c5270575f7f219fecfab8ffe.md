# # Converting strings to time data types

The first task to examine is to take a string and convert it to each of the internal time formats. The `strptime` command will take a string and convert it to a POSIXlt time variable. If you wish to convert a string to a POSIXct data type, you can cast the result of `strptime` using the `as.POSIXct` command. We first focus on converting a string to a POSIXlt data type and provide an example at the end of this section to be converted to a POSIXct data type.

To convert a string to a time data type, the format for the string must be specified, and the formatting options must conform to the ISO C99/POSIX standard. The string includes a sequence of literal characters and a partial list of conversion substrings, which is given in *Table 1*. For example, the `%Y-%m-%d %H:%M:%S` string indicates that the date should look like *2014-05-14 09:54:10* when referring to May 14, 2014 at 9:54 in the morning. A string is assumed to include any number of predefined strings. Anything else is considered to be a literal character that must appear exactly as it appears in the format string.

Once the format string has been specified, the `strptime` command can be used to convert a string into the POSIXlt data type. The arguments for the command are the strings to convert, followed by the format string:

```
> theTime <- c("08:30:00 1867-07-01", "18:15:00 1864-10-27")
> converted <- strptime(theTime,"%H:%M:%S %Y-%m-%d")
> converted
[1] "1867-07-01 08:30:00" "1864-10-27 18:15:00"
> typeof(converted[1])
[1] "list"
> converted[1]-converted[2]
Time difference of 976.5938 days
```

The command also accepts an optional time zone option, as follows:

```
> theTime <- c("08:30:00 1867-07-01", "18:15:00 1864-10-27")
> converted <- strptime(theTime,"%H:%M:%S %Y-%m-%d", + tz="Canada/Eastern")
> converted
[1] "1867-07-01 08:30:00 EST" "1864-10-27 18:15:00 EST"
> converted[1]-converted[2]
Time difference of 976.5938 days
```

The results returned by the `strptime` command are of the POSIXlt data type. They can be converted using the `as.POSIXct` command:

```
> theTime <- c("08:30:00 1867-07-01", "18:15:00 1864-10-27")
> converted <- strptime(theTime,"%H:%M:%S %Y-%m-%d", tz="Canada/Eastern")
> converted
[1] "1867-07-01 08:30:00 EST" "1864-10-27 18:15:00 EST"
> typeof(converted[1])
[1] "list"
> otherTime <- as.POSIXct(converted)
> otherTime
[1] "1867-07-01 08:30:00 EST" "1864-10-27 18:15:00 EST"
> typeof(otherTime[1])
```