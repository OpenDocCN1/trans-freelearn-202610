To uncompress a tar.bz2 file, use two steps:

```
bunzip2 fileArchive.tar.bz
```

```
tar xvf fileArchive.tar
```

# Compression options

When compressing and uncompressing, there are many other options you should take into consideration in order to make your data cleaning job easier:

+   - Do you want to compress a file and also keep the original? By default, most compression and archiving programs will remove the original file. If you want to keep the original file and also create a compressed version of it, you can usually specify this.
+   - Do you want to add new files to an existing compressed file? There are options for this in most archiving and compression programs. Sometimes, this is called **updating** or **replacing**.
+   - Do you want to encrypt the compressed file and require a password to open it? Many compression programs provide an option for this.
+   - When uncompressing, do you want to overwrite files in the directory with the same name? Look for a **force** option.

Depending on which compression software you are using and what its options are, you can use many of these options to make the job of dealing with files easier. This is especially true with large files — either large in size or large in number!

# Which compression program should I use?

The concepts in this section on archiving and compression are widely applicable for any operating system and any type of compressed data files. Most of the time, we will be downloading compressed files from somewhere, and our main concern will be uncompressing these files efficiently.

However, what if you are creating compressed files yourself? What if you need to uncompress a data file, clean it, then recompress it and send it to a coworker? Or what if you are given a choice of files to download, each in a different compression format: zip, bz2, or gz? Which format should you choose?

Assuming that we are in an operating environment that allows you to work with multiple compression types, there are a few rules of thumb for what the various strengths and weaknesses of the different compression types are.