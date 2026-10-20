Your terminal should look similar to the following:

![](bbox=[0.128, 0.13, 0.835, 0.595])

Figure 1.2 - Output from the git diff command

It is good practice to include a .gitignore file in every folder. The .gitignore file contains the names of files and folders to be ignored by Git. This way, you can add and commit all the files in your folder without the fear of committing files like .env.

To include a .gitignore file, run the following command in your terminal:

```
$ touch .gitignore
```

To exempt a file from being tracked by Git, add it to the .gitignore file as follows:

```
$ echo ".env" >> .gitignore
```