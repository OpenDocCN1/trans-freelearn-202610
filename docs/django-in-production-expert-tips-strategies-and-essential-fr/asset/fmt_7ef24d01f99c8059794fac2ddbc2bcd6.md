
1. `<branch type>/<ticket or issue number>/<branch name>`: Here, we start the branch name with the kind of work it is addressing, such as `feature`, `bugfix`, and `improvement`. Then we would add the issue number or the JIRA ticket number followed by a standard branch name. For example, if we are doing a feature and our JIRA number is DJ-112, which would add a comment feature to the blog, then our branch name should look like `feature/DJ-112/integrating-comments`.

2. `<developer or team name/<branch type>/<branch name>`: Here, we would start the branch name with the team or developer name who is working on the branch, followed by the type of work, such as `feature` or `bugfix`. For example, if I am working on adding a comment feature then the branch name would look something like this: `argo/feature/integrating-comments`.

Developers can discuss and decide on any other naming convention that fits well within the team, but it is important to choose a particular pattern and only allow the following pattern to be used while working in the codebase. This can be enforced using local Git hooks and server hooks.

* **Using protected branches**: There should be protected branches in your project where no one can directly push any code. The only way any code can go to these branches should be via PRs. These branches should be used for production deployment or creating tags that are used to deploy to production.

* **Using a branching workflow**: There are numerous strategies developed by different companies that explain the branching strategy. You can read through different famous branching strategies and decide on which branching strategy to follow. Here are a few articles that you can explore:
    1. Here is an article from the Git official website: [https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows).
    2. Here is the link to one of the earliest Git workflows by *Vincent Driessen* that is still referred to across organizations: [https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/).
    3. Here is the GitHub workflow: [https://docs.github.com/en/get-started/quickstart/github-flow](https://docs.github.com/en/get-started/quickstart/github-flow).
    4. Here is the GitLab workflow: [https://docs.gitlab.com/ee/topics/gitlab_flow.html](https://docs.gitlab.com/ee/topics/gitlab_flow.html).

Once we have decided on our branch workflow, we need to focus on the most important Git command, `git commit`.

## Following good practices while using git commit

Once our code changes are done, we will use a `git commit` command. Every `git commit` command can be considered as a log book that can be used to identify code changes made in the past. Hence, we must write proper commit messages for every commit so that our Git history is relevant and it is easy to identify code changes.

Commit messages are one of the most important elements in the SDLC. They are used to identify any change made to the code. Developers must write appropriate commit messages for every commit they make to create maintainable code. Let us discuss a few standard commit message formats that are widely used in the industry.

### Conventional Commits

The Conventional Commit documentation states the following:

*The Conventional Commits specification is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of.*

The Conventional Commits guidelines suggest that the commit message should be structured as follows:

```
<type>[optional scope]: <description>
[optional body]
[optional footer(s)]
```

This helps to easily identify what type of work the code change reflects, and you can also write automated tools and workflows based on the commit messages. The `<type>` mentioned in the commit message helps in identifying what type of change the particular commit is introducing. `scope` defines the scope of the change. And finally, in the `description` block, we add a short description of the overall change. The conventional commit documentation provides an appropriate example to explain different types of commit messages. For further details please visit [https://www.conventionalcommits.org/](https://www.conventionalcommits.org/).

### Use JIRA and GitHub Issues in commit messages

Project management tools are used to track the progress of the development tool. For example, all open source projects hosted in GitHub use GitHub Issues to track any feature request/bug. When we add GitHub issue numbers to our commit messages, GitHub then automatically links the code commit and the issue. This helps in aligning the team to actively track any progress for a given task. Similarly, revisiting the code commit history helps to get context on the what, why, and how of any code change.

If your team uses JIRA or some other tool for project management, then adding the JIRA ID to the commit message helps in identifying and tagging issues faster and better. There are different formats used in the industry and it is best to discuss within the team to finalize the commit message format that suits your team well. Let us look at one of the common formats used:

```
<type> [<JIRA ID>]: <Short description>
```

Read more

For more details, please visit the official documentation of GitHub on how we can use GitHub Issues efficiently: [https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues). To integrate JIRA or other project management tools using GitHub, visit [https://github.blog/2019-10-14-introducing-autolink-references/](https://github.blog/2019-10-14-introducing-autolink-references/).

As the team grows and the project evolves, it is important to add automated tools that can enforce commit message formats. There are a few tools that can help you enforce a good commit message structure. Readers are advised to go through the tool’s official documentation and understand the tools that suit them better:

* `commitlint` ([https://github.com/conventional-changelog/commitlint](https://github.com/conventional-changelog/commitlint))
* `gitlint` ([https://github.com/jorisroovers/gitlint](https://github.com/jorisroovers/gitlint))
* `commitizen` ([https://github.com/commitizen-tools/commitizen](https://github.com/commitizen-tools/commitizen))
* `cz-cli` ([https://github.com/commitizen/cz-cli](https://github.com/commitizen/cz-cli))

Now, let us learn about a few tools that can help developers work with Git with ease.

## Tools with Git

There are plenty of tools available in the market that can help us work better with version control. Let us look into the top tools that one can explore:

* `git-sim` ([https://github.com/initialcommit-com/git-sim](https://github.com/initialcommit-com/git-sim)): When you are a beginner, it can be daunting to understand what different commands are doing in Git. git-sim is a command-line visualizer that can help beginners understand what basic commands are doing.
* **GitHub Desktop**: GitHub Desktop is a GUI for Git. It is platform agnostic and can be used for different code hosting platforms such as GitLab and Bitbucket.
* **GitKraken**: GitKraken is one of the oldest Git GUIs and also one of the most popular GUI tools available in the market.

We have learned how to create a new branch, and then make commits. We saw the good practices on how the branch name should look and what would be a good commit message, but how can we ensure that these guidelines are followed by the developers? That is where the concept of Git hooks comes into the picture.

## Integrating Git hooks into a Django project
