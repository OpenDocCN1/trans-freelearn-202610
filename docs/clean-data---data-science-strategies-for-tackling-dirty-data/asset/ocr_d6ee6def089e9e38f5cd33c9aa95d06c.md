# How to compress files

There are numerous ways of creating compressed files, and which one you choose will depend on the operating system you are using and what compression software you have installed. On OSX, for example, any file or folder (or group) can easily be compressed by selecting it in Finder, right-clicking on it, and choosing Compress from the menu. This action will create a compressed (.zip extension) file in the same directory as the original file. This is shown in the following image:

![](bbox=[0.37, 0.275, 0.63, 0.45])

# How to uncompress files

The collection step of the data science process will often include downloading compressed files. These might be delimited text files, such as the ones we described earlier in this chapter, or they might be files containing multiple types of data files, such as spreadsheets or SQL commands used to build a database.

In any case, uncompressing a file returns the data to a state where we can use it to accomplish our goals. How do we know which program to use to uncompress the file? The first and biggest clue is the file's extension. This is a key tip-off as to what compression program created the file. Knowing how to uncompress the file is dependent on knowing how it was compressed.

In Windows, you can see the installed program that is associated with your file extension by right-clicking on the file and choosing Properties. Then, look for the Open With option to see which program Windows thinks will uncompress the file.

The remainder of this section will outline how to use command-line programs on OSX or a Linux system.