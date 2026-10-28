human-readable format:

```
```r
> save(a,file="a.RData",ascii=TRUE)
> save.image(" wholeworkspace.RData",ascii=TRUE)
> help(save)
```
```

As an alternative to the help command, the ? operator can also be used to get the help page for a given command. An additional command is the help.search command that is used to search the help files for a given string. The ?? operator is also available to perform a search for a given string.

The information in a file can be read back into the workspace using the load command:

```
```r
> load("a.RData")
> ls()
[1] "a"
> a
[1] 19
```
```

Another question that arises with respect to a variable is how it is stored. The two commands to determine this are mode and storage.mode. You should try to use these commands for each of the data types described in the following subsections. Basically, these commands can make it easier to determine whether a variable is a numeric value or another basic data type.

The previous commands provide options for saving the values of the variables within a workspace. They do not save the commands that you have entered. These commands are referred to as the history within the R workspace, and you can save your history using the savehistory command. The history can be displayed using the history command, and the loadhistory command can be used to replay the commands in a file.

The last command given here is the command to quit, q(). Some people consider this to be the most important command because without it you would never be able to leave R. The rest of us are not sure why it is necessary.