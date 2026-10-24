

Finally, the reviewers will review, approve, or reject the changes (they can also merge directly), as illustrated in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_209_1.png)

8. The last step in our process is to merge the code of the `Feature1` branch into that of the master branch, `master`. To perform this merge, we will first load our working directory with the `master` branch, then merge the `Feature1` branch to the current branch, which is `master`.

To do this, we will execute the following commands:

```
git checkout master
git merge Feature1
```

The execution of these commands is displayed as follows:

![](img/d3344b39c888aab0b832625465f837c9_210_0.png)

At the end of its execution, the code of the `master` branch thus contains the changes made in the `Feature1` branch.

We have just seen the use of branches and their merges in Git; now, let’s look at the Gitflow branch pattern and its utility.

## Branching strategy with Gitflow

When we start using branches in Git, the question that often arises is: _Which is the right branch strategy to use_? In other words, what is the key that we need to isolate our code—is it by environment, functionality, theme, or release?

This question has no universal answer, and the management of branches within a project depends on the context of the project. However, there are branch strategy patterns that have been approved by several communities and a multitude of users that allow a better collaboration process in a project under Git.

I suggest we have a closer look at one of these branch patterns, which is Gitflow.

### The Gitflow pattern

One of these patterns is **Gitflow**, developed by _nvie_, which is very popular and has a very easy-to-learn Git workflow.

A branch diagram of Gitflow is shown here:

![](img/d3344b39c888aab0b832625465f837c9_211_0.png)

Figure 6.42 – Gitflow diagram

Let’s look at the details of this workflow, as well as the purpose of each of these branches in their order of creation, as follows:

- 1. First of all, we have the `master` branch, which contains the code that is currently in production. No developer is working directly on it.
- 2. From this `master` branch, we create a `develop` branch, which is the branch that will contain the changes to be deployed in the next delivery of the application.
- 3. For each of the application’s functionalities, a `feature/<name of the feature>` branch is created (/ will hence create a `feature` directory) from the `develop` branch.
- 4. As soon as a feature is finished being coded, the branch of the feature is merged into the `develop` branch.
- 5. Then, as soon as we want to deploy all of the latest features developed, we create a `release` branch from `develop`.
- 6. The `release` branch content is deployed in all environments successfully.
- 7. As soon as the deployment in production has taken place, the `release` branch is merged with the `master` branch, and, with this merge operation, the `master` branch contains the production code. Hence, the code that is on the `release` branch and feature branch is no longer necessary, and these branches can be deleted.
- 8. If a bug is detected in production, we create a `hotfix/<bug name>` branch; then, once the bug is fixed, we merge this branch into the `master` and `develop` branches to propagate the fix on the next branches and deployments.

After seeing the workflow of the Gitflow pattern, we’ll discuss the tools that facilitate its implementation.

## Gitflow tools

To help teams and developers to use Gitflow, _nvie_ created a Git override command-line tool that allows you to easily create branches according to the workflow step. This tool is available on the _nvie_ GitHub page here: `https://github.com/nvie/gitflow`.

On the other hand, there are also Git graphical tools that support the Gitflow model, such as GitKraken (`https://www.gitkraken.com/`) and Sourcetree (`https://www.sourcetreeapp.com/`). These tools allow us to use the Gitflow process via a graphical interface, as shown in the following screenshot, with the use of Sourcetree with a `Git-flow` feature that allows us to create a branch using the Gitflow pattern.

The following screenshot shows the configuration of Sourcetree for Gitflow pattern branch naming:

![](img/d3344b39c888aab0b832625465f837c9_212_0.png)

Figure 6.43 – Sourcetree Git-flow tool

Then, after this configuration, we can create a branch easily with this name, as shown in the following screenshot:

![](img/d3344b39c888aab0b832625465f837c9_213_0.png)

Figure 6.44 – Creating a Sourcetree branch

As shown in the preceding screenshots, we use Sourcetree and the graphical interface that allows us to create branches intuitively.

> **Note**
>
> For more documentation on Gitflow and its process, read this article:
> https://jeffkreeftmeijer.com/git-flow/.

To get started with Gitflow, I suggest you get used to small projects, then you will see that the mechanism is the same for larger projects. In this section, we saw the process of using Git's command lines with its workflow. Then, we talked about the management of development branches, and finally, we went a little further into branches with Gitflow, which allows the simple management of branches and a better development workflow with Git.

## Summary

Git is today an essential tool for all developers; it allows us to use command lines or graphical tools and share and version code for better team collaboration.

In this chapter, we saw how to install Git on the different OSes and an overview of the main command lines. Then, through a small lab, we saw the Git workflow with the application of Git command lines. Finally, we presented the isolation of code with the implementation of branches and the use of the Gitflow pattern, which gives a simple model of branch strategy.

In the next chapter, we will talk about CI/CD, which is one of the key practices of the DevOps culture.

## Questions

- 1. What is Git?
- 2. Which command is used to initialize a repository?
- 3. What is the name of the Git artifact for saving part of the code?
- 4. Which Git command allows you to save your code in the local repository?
- 5. Which Git command allows you to send your code to the remote repository?
- 6. Which Git command allows you to update your local repository from the remote repository?
- 7. Which Git mechanism is used for Git code isolation?
- 8. What is Gitflow?

## Further reading

If you want to know more about Git, here are some resources:

- Git documentation: https://www.git-scm.com/doc
- Pro Git book: https://git-scm.com/book/en/v2
- Mastering Git book: https://www.packtpub.com/application-development/mastering-git
- Set up Git: https://try.github.io/
- Learn Git Branching: https://learngitbranching.js.org/
- Atlassian Git tutorials: https://www.atlassian.com/git/tutorials
- git-flow cheat sheet: https://danielkummer.github.io/git-flow-cheatsheet/

# 7 Continuous Integration and Continuous Delivery

One of the main pillars of **development-operations (DevOps)** culture is the implementation of **continuous integration (CI)** and deployment processes, as we explained in _Chapter 1, The DevOps Culture and Infrastructure as Code Practices_.

In the previous chapter, we looked at the use of Git with its command lines and usage workflow, and in this chapter, we will look at the important role Git has in the CI/CD workflow.

**CI** is a process that provides rapid feedback on the consistency and quality of code to all members of a team. It occurs when each user's code commit retrieves and merges the code from a remote repository, compiles it, and tests it.

**Continuous delivery (CD)** is the automation of the process that deploys an application in different stages (or environments).

