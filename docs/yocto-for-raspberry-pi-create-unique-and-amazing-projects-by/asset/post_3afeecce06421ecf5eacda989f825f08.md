
## Working with Git

A project in Git is called a repository. Most of the work can be done locally by cloning the main repository to your preferred working directory. This is one of the reasons why Git is often considered very fast, when compared with other version control systems that need network access for many operations.

Let's go through some basic examples, which will teach you how to:

- Import an existing project to a Git repository
- Create your first commit
- Convert to a shared repository
- Clone a local version of the shared repository
- Create a work branch
- Push a commit to your shared repository
- Update the cloned repository with the latest changes

When importing an existing project into a new Git repository, we must first initialize an empty Git repository. This can easily be done within the existing source tree of our project:

```
$ tar -zxf my_packt_project_ch_11_01.tar.gz
$ cd my_packt_project
$ git init
Initialized empty Git repository in  /path/to/repo/my_packt_project/.git/
```

When initializing a new Git repository, a new directory, .git, is created. This directory contains all Git-related information about your new repository. Among other things, it contains the main Git configuration file, config, which contains the Git settings for your project. Another frequently used file within this directory is HEAD, which contains a reference to your current branch.

After the empty Git repository has been created, we can continue with adding all the content of our project to the new repository. A useful command to obtain the current status of your branch and repository is git status. When Git isn't aware of a file, it will show up under Untracked files, as shown here:

```
$ git status
On branch master
Initial commit
Untracked files:
  (use "git add <file>..." to include in what will be committed)
Makefile
README
my_packt_project.c
nothing added to commit but untracked files present (use "git add" to track)
```

We can add all files and/or changes with a single command, but be aware that it will add build output as well. So, when using git add --all, make sure that you have removed unwanted files first. Then, we can use git status again to check whether all the expected files are listed:

```
$ git add --all
$ git status
```

```
On branch master
Initial commit
Changes to be committed:
(use "git rm --cached <file>..." to unstage)
new file:   Makefile
new file:   README
new file:   my_packt_project.c
```

Before we can submit our changes and create our first commit, we need to set up some basic Git configurations. At the minimum, Git requires you to set up your name and e-mail:

```
$ git config user.email "your_email@packtpub.com"
$ git config user.name "Forename Surname"
```

If you want to set up a global e-mail and name for all your Git repositories, you can specify the --global option, like this:

```
$ git config --global user.email "your_email@packtpub.com"
$ git config --global user.name "Forename Surname"
```

To verify the settings, type this:

```
$ git config user.email
your_email@packtpub.com
$ git config user.name
Forename Surname
```

### Tip

Global Git configurations are stored in /home/$USER/.gitconfig and local ones are stored in /path/to/git/repository/.git/config. By default, changes in the local repositories' Git configuration will override changes in your global Git configuration.

When committing a change in Git, the git commit command is used. It records changes to the repository and thereby makes it possible to track the commit using the unique SHA-1 hash generated for the commit. An historical overview of a branch can be seen by typing git log [branch].

```
$ git commit -m "My initial commit"
[master (root-commit) f01bdf2] My initial commit
3 files changed, 11 insertions(+)
create mode 100644 Makefile
create mode 100644 README
create mode 100644 my_packt_project.c
$ git log
commit f01bdf29a2c9df1c2417542c5531f90e04c973a
Author: Forename Surname <your_email@packtpub.com>
Date:   Sun Jan 1 00:00:00 2016 +0100
My initial commit
```

When multiple developers work on the same project, each user uses their own local clone of the repository. Git will by default reject changes pushed to a repository that isn't a bare repository. We will now go through a simple example that will:

- Convert our repository to a shared repository
- Clone the shared repository

A shared repository is normally stored on a server and the repository must be created as a bare Git repository. This is done by using the `--bare` flag when initializing the Git repository or while cloning. Since we worked with a non-shared repository in our earlier examples, we will convert it to a shared repository using `git clone --bare`, and we can then remove our old repository and continue working with clones of the bare repository instead.

```
$ cd /path/to/my/workdir
$ git clone --bare /path/to/my_packt_project my_packt_project.git
$ rm -rf /path/to/my_packt_project
```

### Tip

A bare repository is basically a repository that doesn't contain a working directory. This will prevent developers from making changes to the repository. According to naming conventions, a bare Git repository should be named `name_of_my_repo.git`.

We now have a bare repository that can be used by a collaboration of developers. Next, we continue with cloning a local copy of the bare repository, which we can use when developing our new features. By default, you will be in a branch called `master` when cloning a repository. It's possible to do all our work in the master branch; since this is only a local clone, we will not destroy anything for other developers. But it's strongly recommended to create a **working branch** and make all your changes in that branch instead. This way, the master branch is always in a stable and known state. When you are an advanced Git user, you might work on several features in parallel and also might want to integrate new features that other developers have worked on through the master branch. In other words, it's a good habit to create separate branches for all your features.

```
$ git clone /path/to/my_packt_project.git
Cloning into 'my_packt_project'
$ cd my_packt_project
```

Next, we will create a new branch called `my_new_feature`, move it, and develop a tiny new feature for the project. It's possible to use `git branch` to list all available local branches; if you add the `-a` flag when listing branches, all remote branches will be listed as well.

### Tip

A remote branch is a branch that exists in a shared repository that has been cloned from. The asterisk (*) helps the user locate the current branch.

```
$ git branch
* master
$ git branch my_new_feature
$ git checkout my_new_feature
Switched to branch 'my_new_feature'
```

```
$ git branch
master
* my_new_feature
$ git branch -a
master
* my_new_feature
remotes/origin/HEAD -> origin/master
remotes/origin/master
```

We are now in our new branch and it's time to make some modifications. In this example, we will add a new target, clean, to our makefile. Remember to use git add in order to add your changes to the index before committing them. Use git diff before adding the file if you want to list your changes.

### Note

If you have already added a file using git add, you can check your changes before committing them by using git diff --cached.

```
$ vim Makefile
$ git diff
diff --git a/Makefile b/Makefile
index c6466e0..d986116 100644
--- a/Makefile
+++ b/Makefile
@@ -1,2 +1,5 @@
all:
$(CC) my_packt_project.c -o my_packt_project
+
+clean:
+rm -f my_packt_project
$ git add Makefile
$ git commit -m "Makefile: Add a new target"
[my_new_feature 7c3d896] Makefile: Add a new target
1 file changed, 3 insertions(+)
```

The last step for us is to send our new change to the shared repository, update our local master with the new changes, and then remove our feature branch, since it will no longer be required. When pushing a change to our shared repository, we can use the predefined remote origin; by default, the origin is set to point to the repository we cloned from. When pushing a change, we need to specify which revision we will send and to which branch. In most situations, we will want to send the entire branch; if so, we can use HEAD to obtain all changes from the current branch, like this:

```
$ git push origin HEAD:master
Counting objects: 5, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 383 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To /path/to/my_packt_project.git
f01bdf2..7c3d896  HEAD -> master
$ git checkout master
Switched to branch 'master'
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
(use "git pull" to update your local branch)
