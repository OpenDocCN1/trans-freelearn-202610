pine, 34.8
oak, 25.3

The first column is the character data, and the second column is the numeric data. The information on each line is separated by a comma. The scan command assumes that the information is separated using white space (spaces and tabs), so in this case, we have to specify that a comma is used as the separator within the file. In the following example, the file is read, and the format is given using the `what` argument:

```
> x <- scan("trees.csv",what=list("character","double"),sep=",")
Read 5 records
> x
[[1]]
[1] "pine" "pine" "oak" "pine" "oak"

[[2]]
[1] "28.8" "27.3" "45.8" "34.8" "25.3"
```

Another method for entering data is to use the `data.entry` command. The command will open up a graphical interface if it is available on your system. The details can vary depending on your operating system and the graphical interface that you are using.