

From the preceding execution, we can see that a Readme.md file is created and it will also integrate to commit.

The last operation to do is validate the change by archiving it in the local repository, and for this, we execute the following command:

```
git commit -m "Add file readme.md"
```

We make a commit with a description, for keeping track of changes. Here is a screenshot of the preceding command:

![](img/d3344b39c888aab0b832625465f837c9_201_1.png)

## Archiving on the remote repository

To archive local changes and allow the team to work and collaborate on this code as well, we will push our commit in the remote repository by executing the following command:

```
git push origin master
```

We indicate to the `git push` command the alias and the branch of the remote repository. During the first execution of this command, we will be asked to authenticate to the Azure DevOps repository, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_202_0.png)

Figure 6.29 – git push on Azure DevOps

And after authentication, the push is executed, as follows:

```
C:\Users\user\BookProject>git push origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 275 bytes | 275.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: Analyzing objects... (3/3) (6 ms)
remote: Storing packfile... done (324 ms)
remote: Storing index... done (148 ms)
To https://dev.azure.com/BookLabs/BookDemo/_git/BookDemo
 * [new branch]      master -> master
```

Figure 6.30 – git push result

The remote repository is up to date with our changes, and in the Azure Repos interface, we can see the code of the remote repository. The following screenshot shows the directory in Azure Repos that contains the added file:

Figure 6.31 – Azure Repos new file added

That’s it: we have performed the first major step of the Git process, which consists of initializing a repository and pushing code into a remote repository. The next step is to make changes to the code by another team member.

## Cloning the repository

When another member wants to retrieve the entire remote repository code for the first time, they perform a clone operation, and to do so, they must execute this command:

```
git clone <repository url>
```

The execution of this command performs these actions, as follows:

1. Creates a new directory with the name of the repository.
2. Creates a local repository with its initialization and configuration.
3. Downloads the remote code.

The team member can therefore modify and make changes to the code.

## Updating the code

When the code is modified and the developer updates their changes on the remote repository, they will perform exactly the same actions as when the remote repository was initialized, with the execution of the following commands:

```
git add .
git commit -m "update the code"
git push origin master
```

We added the files to the next commit, created a commit, and pushed the commit to the remote repository.

The last remaining step is the retrieval of updates by other members.

## Retrieving updates

When one member updates the remote repository, it is possible to retrieve these updates and update our local repository with the changes by running the `git pull` command, as follows:

```
git pull origin master
```

The following screenshot shows the execution of this `git pull` command:

![](img/d3344b39c888aab0b832625465f837c9_204_0.png)

Figure 6.32 – git pull command

From the preceding execution, we indicate the origin alias and master branch to be updated in the local repository, and its execution displays the pushed commits. At the end of the execution of this command, our local repository is up to date.

For the rest of the process with the code update, this is the same as we saw earlier in the Updating the code section.

We have just seen the simple process of using Git, but it is also possible to isolate its development with the use of branches.

## Isolating your code with branches

When developing an application, we often need to modify part of the code without wanting to impact the existing stable code of the application. For this, we will use a feature that exists in all VCSs that allows us to create and manage branches.

The mechanics of using the branches are quite simple, as outlined here:

1. From a branch, we create another branch.
2. We develop this new branch.
3. In case we work in teams, create a PR for a discussion about the code changes. If reviewers agree with these changes, they approve the changes' code.
4. To apply the changes made on this new branch to the original branch, a merge operation is performed.

Conceptually, a branch system is represented in this way:

![](img/d3344b39c888aab0b832625465f837c9_205_0.png)

Figure 6.33 – Branch diagram

In this diagram, we see that branch B is created from branch A. These two branches will be modified by development teams for adding a new feature to our application. But in the end, branch A is merged with branch B, and hence the branch B code changes.

Now that we have seen the branch bases, let’s look at how to use branches in Git by following these steps:

1. The purpose of this lab is to create a `Feature1` branch from the `master` branch. Then, after some changes on the `Feature1` branch, we will merge the code changes from the `Feature1` branch to the `master` branch. First, create a `Feature1` branch on the local repository, from `master`, with the help of the following command:

```
git branch Feature1
```

The execution of this command created a new branch, `Feature1`, which contains exactly the same code as the parent branch, `master`.

2. To load your working directory with the code of this branch, we execute the following command:

```
git checkout Feature1
```

From the following output, we can see the switch of the branch:

![](img/d3344b39c888aab0b832625465f837c9_206_0.png)

3. We can also see a list of branches of our local repository with the following command:

```
git branch
```

The output of this command is shown here:

![](img/d3344b39c888aab0b832625465f837c9_206_1.png)

During execution, we can see two branches and the active branch as well.

4. Now, we will make changes to our code on this branch. The update mechanism is identical to everything we have already seen previously, so we will execute the following commands:

```
git add .
git commit -m "Add feature 1 code"
```

5. For the `push` operation, we will specify the name of the `Feature1` branch in the parameter with this command:

```
git push origin Feature1
```

The following screenshot shows the code execution that performs all of these steps:

![](img/d3344b39c888aab0b832625465f837c9_207_0.png)

Figure 6.36 – Adding a Git branch

6. With the preceding commands that we executed, we created a commit on the `Feature1` branch. We then executed the `push` command to publish the new branch as well as its commit in the remote repository. On the other hand, in the Azure Repos interface, we can see our two branches in the **Branches** section, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_207_1.png)

Figure 6.37 – Azure Repos branches list

7. Before merging the code, we create a PR for a discussion about the changes. The following screenshots show PR creation and approval steps in Azure DevOps.
We start by creating a PR, as follows:

![](img/d3344b39c888aab0b832625465f837c9_208_0.png)

Figure 6.38 – Azure Repos: Creating a PR
Then, fill in the form of this PR by selecting the source and target branches, entering a title and description, and choosing reviewers. We can also view the difference in the code that will be merged. The process is illustrated in the following screenshot:

