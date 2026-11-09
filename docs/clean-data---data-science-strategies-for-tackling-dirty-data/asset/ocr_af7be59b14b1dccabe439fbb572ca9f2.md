The ls command shows that a directory is created called ferguson-tweet-ids, and there are two zipped files inside that directory as well: ferguson-indictment-tweet-ids.zip and ferguson-tweet-ids.zip. We really only need to unzip one of these to perform this project, so I chose this one:

```
unzip ferguson-tweet-ids.zip
```

Unzipping this file exposes several manifest text files, as well as a data folder. Inside the data folder is a gzipped file. Unzip it as follows:

```
gunzip ids.txt.gz
```

This yields a file called ids.txt. This is the file we are actually after. Let's explore this file.

To see the size of the file, we can run the wc command. When run from the command prompt as shown here, wc shows how many lines, words, and characters are in the file, in that order:

```
megan$ wc ids.txt
  13238863 13238863 251538397 ids.txt
```

The first number indicates how many lines are in the ids.txt file, just over 13 million. Next, we can peek inside the file with the head command:

```
megan$ head ids.txt
501064188211765249
501064196642340864
501064197632167936
501064196931330049
501064198005481472
501064198009655296
501064198059597824
501064198513000450
501064180468682752
501064199142117378
```

The head command shows the first ten lines of the file so we can see that each line is comprised of an 18-digit tweet ID.