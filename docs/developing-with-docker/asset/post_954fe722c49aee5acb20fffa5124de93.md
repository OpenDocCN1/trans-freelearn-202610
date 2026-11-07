
All subsequent instructions in the Dockerfile will use the image specified in FROM as a base starting point. That's why it's mandatory–a valid Dockerfile must have it at the top. Although you can have multiple FROM instructions in your image, it's probably not a good and recommended practice–while interesting, isn't fully supported. In particular, the nonfinal images that are produced by using multiple FROM are not easily found without some parsing of the build output. Actually, there is an ongoing discussion on Docker's GitHub about removing the support for multiple FROM instructions from a Dockerfile specification.

The FROM instruction takes a tag or digest as a parameter. If you decide to skip them, Docker will assume you want to build your image from the latest tag. As you remember from Chapter 5, Finding Images, it will not always be the latest version of the image you want to build upon. Docker will throw an error during the build if it cannot find a tag or digest you provide.

## MAINTAINER

By using the MAINTAINER instruction, you set the Author field of the generated image. This can be your name, username, or whatever you would like it to be referenced with, as an author of the image that will be created using this Dockerfile. This command can be placed anywhere in a Dockerfile, but the good practice is to place it on top of the file, just after the FROM instruction. This is the so-called non-executing command, which means that it will not make any changes to the generated image. The syntax, again, is very simple:

```
MAINTAINER authors_name
```

## ADD

The ADD basically copies the files from the source on the host into the Docker image's own filesystem at the desired destination. It takes two arguments; the source (<source path or URL>) and a destination (<destination path>):

```
ADD <source path or URL> <destination path >
```

The source path can be a file or a directory relative to the directory in which the build process is going to be started (the build context). This means you cannot have, for example, `../../config.json` placed as a source path parameter of the `ADD` instruction. The build context can be changed using the `PATH` option in the `docker build` command and the source path is relative to the build context.

The source can contain wildcards. These are the same as in conventional filesystem: * for any text string, or ? for any single character.

Consider the following examples:

- `ADD myFiles* /myDirectory/`: This will add all the files starting with myFiles
- `ADD file?.txt /myDirectory/`: This will replace with any single character so that it will match `file1.txt`, `fileB.txt`, and so on.

![](img/0c87321919fa245e22f056c67ec70003_140_0.png)

The destination path, on the other hand, cannot contain wildcards.

You can specify multiple source paths, separated with a comma. All of them must be relative to the building context, the same as if you have just a single source path. If your source or destination paths contain spaces, you will need to use a special syntax, adding the square brackets around:

```
ADD ["<source path or URL>" "<destination path>"]
```

A trailing slash / is quite important when adding files or directories to an image. If the source path doesn't end with a trailing slash, it will be considered as a single file and just copied into the destination. If the source path ends with a trailing slash, it will be considered as a directory; all of its contents will then be copied into the destination path, but the directory itself will not be created at the destination path.

If the source path points to the compressed archive in one of the common formats such as `zip`, `tar`, and so on, it will be decompressed into the destination path. Docker doesn't recognize an archive by the filename, it checks the contents of the file.

![](img/0c87321919fa245e22f056c67ec70003_141_0.png)

If the archive is damaged or unreadable by Docker in another way, it will not be extracted and you will not be given an error message—the file will be just copied into the destination path.

The same trailing slash rules apply to the destination path: if it ends with a trailing slash, it means that it's a directory. Otherwise, it will be considered as a single file. This gives you great flexibility when constructing filesystem contents of your image—you can add files into directories or just add whole directories.

![](img/0c87321919fa245e22f056c67ec70003_141_1.png)

Note that when using ADD with more than one source file, the destination must be a directory and end with a slash /.

The ADD command is not only about copying files from the local filesystem—you can use it to get the file from the network. If the source is a URL, then the contents of the URL will be automatically downloaded and placed at the destination. Note that file archives that were downloaded from the network will not be decompressed. Again, the trailing slash is important when downloading files—if the destination path ends with a slash, the file will be downloaded into the directory. Otherwise, the downloaded file will be just saved under the name you provided as the destination path.

The <destination directory> is either an absolute path or a path that is relative to the directory specified by the WORKDIR instruction (we will cover it in a while). The source (or multiple sources) will be just copied into the destination specified. Consider the following example:

- `ADD config.json projectRoot/` will add the `config.json` file to `<WORKDIR>/projectRoot/`
- `ADD config.json /absoluteDirectory/` will add the `config.json` file to the `/absoluteDirectory/`

When it comes to the ownership of the files created in the image, they will always be created with the user ID (UID) 0 and group ID (GID) 0. Permissions will be the same as in the source file, unless it's a file downloaded from the remote URL. In this case, it will get permissions value 600 (only the owner can read and write the file). If you need to change these values (ownership or permissions), you will need to provide more instructions in your Dockerfile, after the ADD instruction.

![](img/0c87321919fa245e22f056c67ec70003_142_0.png)

If the files that you need to add to the image are placed on the URL that needs authentication, the ADD instruction will not work–you will need to use the shell command to download the file, such as wget or curl.

## COPY

The COPY instruction will copy new files or directories from <source path> and adds them to the filesystem of the container at the path <destination path>.

It's very similar to ADD instructions, even the syntax is identical:

```
COPY <source path or URL> <destination path >
```

The same rules from ADD apply to COPY–all source paths must be relative to the context of the build. Again, the presence of the trailing slash at the end of source and destination path is important. If it's present, the path will be considered as a directory, otherwise it will be treated as a file.

Of course, as in ADD, you can have multiple source paths. If source or destination paths contain spaces, you will need to wrap them into square brackets:

```
COPY ["<source path or URL>" "<destination path>"]
```

The <destination path> is an absolute path (it begins with a slash), or a path relative to the path specified by the WORKDIR instruction.

As you can see, the functionality of COPY is almost the same as that of the ADD instruction with one difference. COPY supports only the basic copying of local files into the container. On the other hand, ADD features some more, such as archive extraction, downloading files through URL, and so on. Docker's best practices say, that you should prefer COPY if you do not need those additional features of ADD. The Dockerfile will be cleaner and easier to understand thanks to the transparency of the COPY command.

There is one common, important aspect for both ADD and COPY instructions, a cache. Basically, Docker caches the files that go into the image during the build. The contents of the file or files in the image is examined and a checksum is calculated for each file. During the cache lookup, the checksum is compared against the checksum in the existing images. If anything has changed in the file(s), such as the contents and metadata, then the cache is being invalidated. Otherwise, if the source file has not changed, then an existing image layer is being reused.
