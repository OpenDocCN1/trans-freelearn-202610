
## Compression with zip, gzip, and bzip2

zip, gzip and bzip2 are the most common compression programs. Their uncompression partners are called Unzip, Gunzip, and Bunzip2, respectively.

The following table shows a sample command-line for compressing and uncompressing in each of these programs.

| | Compress | Uncompress |
|---|---|---|
| Zip | `zip filename.csv filename.zip` | `unzip filename.zip` |
| gzip | `gzip filename.csv filename.gz` | `gunzip filename.gz` |
| bzip2 | `bzip2 filename.csv filename.bz2` | `bunzip2 filename.bz2` |

Sometimes, you will see a file that includes both a .tar and a .gz extension or a .bz2 extension like this: somefile.tar.gz. Other combinations that are common include: .tgz and .tbz2, as in somefile.tgz. These are files that have been tarred (archived) first and then compressed using gzip or bzip2. The reason for this is that gzip and bzip2 are not archiving programs; they are only compressors. Therefore, they can only compress a single file (or file archive) at a time. As tar's job is to make multiple files into single files, these programs are found together very often.

The tar program even has a built-in option that will gzip or bzip2 a file immediately after tarring it. To gzip the newly created .tar file, we can simply add z to the preceding tar command, and modify the filename:

```
tar cvzf fileArchive.tar.gz reallyBigFile.csv anotherBigFile.csv
```

Or, you can do this in two steps:

```
tar cvf fileArchive.tar reallyBigFile.csv anotherBigFile.csv
```

```
gzip fileArchive.tar
```

This sequence of commands will create the fileArchive.tar.gz file:

To uncompress a tar.gz file, use two steps:

```
gunzip fileArchive.tar.gz
```

```
tar xvf fileArchive.tar
```

These steps also work for bzip2 files:

```
tar cvjf fileArchive.tar.bz2 reallyBigFile.csv anotherBigFile.csv
```

To uncompress a tar.bz2 file, use two steps:

```
bunzip2 fileArchive.tar.bz2
```

```
tar xvf fileArchive.tar
```

## Compression options

When compressing and uncompressing, there are many other options you should take into consideration in order to make your data cleaning job easier:

- Do you want to compress a file and also keep the original? By default, most compression and archiving programs will remove the original file. If you want to keep the original file and also create a compressed version of it, you can usually specify this.
- Do you want to add new files to an existing compressed file? There are options for this in most archiving and compression programs. Sometimes, this is called **updating** or **replacing**.
- Do you want to encrypt the compressed file and require a password to open it? Many compression programs provide an option for this.
- When uncompressing, do you want to overwrite files in the directory with the same name? Look for a **force** option.

Depending on which compression software you are using and what its options are, you can use many of these options to make the job of dealing with files easier. This is especially true with large files — either large in size or large in number!

## Which compression program should I use?

The concepts in this section on archiving and compression are widely applicable for any operating system and any type of compressed data files. Most of the time, we will be downloading compressed files from somewhere, and our main concern will be uncompressing these files efficiently.

However, what if you are creating compressed files yourself? What if you need to uncompress a data file, clean it, then recompress it and send it to a coworker? Or what if you are given a choice of files to download, each in a different compression format: zip, bz2, or gz? Which format should you choose?

Assuming that we are in an operating environment that allows you to work with multiple compression types, there are a few rules of thumb for what the various strengths and weaknesses of the different compression types are.

Some of the factors we use when making a compression decision are:

- The speed of compressing and uncompressing
- The compression ratio (how much smaller did the file get?)
- The interoperability of the compression solution (can my audience easily decompress this file?)

## Rules of thumb

Gzip is faster to compress and decompress, and it is readily available on every OSX and Linux machine. However, some Windows users will not have a gunzip program readily available.

Bzip2 makes smaller files than gzip and zip, but it takes longer to do so. It is widespread on OSX and Linux. Windows users will probably struggle to handle bzip2 files if they have not already installed special software.

Zip is readily available on Linux, OSX, and Windows, and its speed to compress and decompress is not terrible. However, it does not create very favorable compression ratios (other compressors make smaller files). Still, its ubiquity and relative speed (compared to bzip2, for example) are strong points in its favor.

RAR is a widely available archiving and compression solution for Windows; however, its availability for OSX and Linux requires special software, and its compression speed is not as good as some of the other solutions.

Ultimately, you will have to decide on a compression standard based on the particular project you are working on and the needs of your audience or user, whether that is yourself, a customer, or a client.

## Data types, nulls, and encodings

This section provides an overview of the most common data types that data scientists must deal with on a regular basis and some of the variations between these types. We also talk about converting between data types and how to safely convert without losing information (or at least understanding the risks beforehand).

This section also covers the mysterious world of empties, nulls, and blanks. We explore the various types of missing data and describe how missing data can negatively affect results of data analysis. We will compare choices and trade-offs for handling the missing data and some of the pros and cons of each method.

As much of our data will be stored as strings, we will learn to identify different character encodings and some of the common formats you will encounter with real-world data. We will learn how to identify character encoding problems and how to determine the proper type of character encoding for a particular dataset. We will write some Python code to convert from one encoding scheme to another. We will also cover the limitations of this strategy.

## Data types

Whether we are cleaning data stored in a text file, a database system, or in some other format, we will start to recognize the same types of data make an appearance over and over: numbers of various kinds, dates, times, characters, strings of characters, and more. The upcoming sections describe a few of the most common data types, along with some examples of each.

## Numeric data

In this section, we will discover that there are many ways to store a number, and some of them are easier to clean and manage than others. Still, numbers are fairly straightforward compared to strings and dates, so we will start with these before moving on to trickier data types.

## Integers

Integers, or whole numbers, can be positive or negative, and just like the name implies, they do not have decimal points or fractions. Depending on what kind of storage system the integers are stored in, for example, in a **Database Management System (DBMS)**, we may also have additional information about how large an integer can be stored as well as whether it is allowed to be signed (positive or negative values) or only unsigned (all positive values).

## Numbers with decimals

In our data cleaning work, numbers with a fractional component—such as prices, averages, measurements, and the like—are typically expressed using a decimal point (rather than a numerator/denominator). Sometimes, the particular storage system in place also has rules specifying the number of digits that are allowed to come after the decimal point (**scale**) as well as the total number of digits allowed in the number (**precision**). For example, we say that the number 34.984 has a precision of 5 and a scale of 3.
