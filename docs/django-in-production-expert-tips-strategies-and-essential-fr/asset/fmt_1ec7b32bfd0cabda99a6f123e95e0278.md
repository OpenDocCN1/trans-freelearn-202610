
Git has a way to execute custom scripts whenever a certain important action occurs in Git; these custom event triggers are known as Git hooks. Git provides two types of hooks: client-side hooks run on the developer machine and events such as `commit`, `merge`, and `push`, while server-side hooks run on the remote server and events such as `pre-receive` and `post-receive`. Git hooks reside in the `.git/hooks` folder. One can easily create the Git hooks and integrate them, and different frameworks help us use them. We shall not get into the details of how to add a custom hook manually but rather work with the `lefthook` framework. Though one of the most popular Git hook frameworks for the Python ecosystem is `pre-commit`, we shall not work with `pre-commit`. Rather, we shall learn how to use `lefthook` since it is faster and more flexible.

## Using lefthook

`Lefthook` is a Git hook manager written in Go. Since it is a dependency-free binary, it is easy to install and run in any environment. It can run commands in parallel, which makes it 3x faster than `pre-commit` and faster than most of the Git hook managers available. Lefthook documentation is evolving, but the community is quite active and supportive of resolving any blockers. For more details, check the project repository at [https://github.com/evilmartians/lefthook](https://github.com/evilmartians/lefthook).

Now, let us look into how to integrate `lefthook` into our Django project. The official documentation has all the mentioned details on how to install it: [https://github.com/evilmartians/lefthook/blob/HEAD/docs/install.md](https://github.com/evilmartians/lefthook/blob/HEAD/docs/install.md).

For Mac systems, we can use the following command, using `homebrew`, to install lefthook:

```
brew install lefthook
```

Linux users can install lefthook via the APT package manager:

```
curl -1sLf 'https://dl.cloudsmith.io/public/evilmartians/lefthook/setup.deb.sh' | sudo -E bash
sudo apt install lefthook
```

For Windows users, lefthook can be installed by using scoop ([https://scoop.sh/](https://scoop.sh/)):

```
scoop install lefthook
```

Alternatively, windows users can use winget to install lefthook ([https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli)):

```
winget install evilmartians.lefthook
```

Now, let us integrate lefthook into our project. To learn lefthook, we shall create a simple project:

1. Create a `demo-project` folder and add a new `demo-project/demo_script.py` file with the following code:

```
def hello_world():
    print("hello world")
hello_world()
```

2. Add `git` to the folder by using the following command:

```
git init
```

3. Now, we shall add `lefthook` to our project by using the following command:

```
lefthook install
```

This command will add a new `lefthook.yaml` file to the project folder that will have a bunch of sample commands that are commented.

4. Let us add a sample Git hook that would check the configuration in the generated `lefthook.yaml` file that has sample checks:

```
pre-commit:
  commands:
    demo-echo:
      run: echo "Just Demo command"
    demo-echo2:
      run: echo "Just Demo command 2"
```

5. The `pre-commit` commands run whenever a developer would make a commit. So, when we perform the `git commit` command, we see Lefthook run the following custom hooks:

```
> git commit -am "Init commit"
 lefthook v1.5.5  hook: pre-commit
┃  demo-echo ❯
Just Demo command
┃  demo-echo2 ❯
Just Demo command 2
  ────────────────────────────────────
summary: (done in 0.03 seconds)
✔️  demo-echo
✔️  demo-echo2
[main 30019a4] Init commit
 2 files changed, 9 insertions(+)
create mode 100644 demo_script.py
```

The `echo` command that is used can be easily replaced by any other command that we want to execute.

Lefthook is versatile in nature and can be used to add Git hooks in different programming languages such as Python, Javascript, and Shell Script.

Read more

For more details, visit the official documentation of lefthook that explains all the features it provides: [https://github.com/evilmartians/lefthook/blob/master/docs/usage.md](https://github.com/evilmartians/lefthook/blob/master/docs/usage.md).

The beauty of lefthook is that it is language agnostic, so we can use lefthook to create Git hooks for different languages. Here are a few of the lefthook scripts I generally configure:

* Linting of all the staged files using Black and Flake8
* Enforce the branch name as per the convention
* Enforce the commit message format as per the convention
* Stop any large file to be committed to the repository

Lefthook helps in client-side Git hooks. The other type of Git hook is the server-side hook. Unfortunately, GitHub does not support server-side hooks. But such custom logic can be implemented using GitHub Actions and whenever a developer is not following the guideline, we shall fail the GitHub Actions pipeline.

Now that we have learned how to force the developers to adhere to the guidelines, let us focus on how we can merge our code changes and release the code.

## Using git merge versus git rebase

`git merge` and `git rebase` have their advantages and disadvantages. In *Figure 12.1*, we can see how the history of commits gets affected when we use the `git merge` or `git rebase` command. So, depending upon the use case, developers should choose whether to select `git merge` or `git rebase`. But leaving that option to the developers can also be dangerous, especially if your team has many junior developers. Hence, I generally prefer to choose `git merge` as the default option whenever creating a process in the team since it is easy to understand, so everyone has very little chance of messing up.

The advantages of using `git merge` are the following:

* It preserves the full repo history
* It makes it easy to resolve conflicts between different branches
* It makes it easy to revert any code change

![Figure 12.1: Image illustrating git merge versus git rebase](image/Figure_12.1_B18867.jpg)

Figure 12.1: Image illustrating git merge versus git rebase

GitHub and other similar platforms give another option to choose from while merging using the UI (i.e., `Create a merge commit` or `Squash and merge`). Both have their advantages and disadvantages, and it depends on the situation. The biggest advantage of selecting `Squash and merge` is that it keeps the commit history clean, but the disadvantage is that you lose out on all the history of the commits made. While the `Create a merge commit` option would simply add a new merge commit and you would have the past commit history intact, I prefer using the `Create a merge commit` option for most of my use cases.

Once the code is merged, we are ready to deploy our code and release our feature to the users.

## Performing code release

Shipping your code to production is the most fascinating experience for any developer. Watching one’s code used by tens, thousands, or even millions of users is an absolute pleasure and joy to have. Hence the code release process must be defined well. Every team can decide on their code release process; one can have a manual deployment in each server or have a sophisticated `continuous deployment` (`CD`) process.

I generally prefer to build an automated one-click deployment process for services. In [*Chapter 13*](#B18867_13.xhtml#_idTextAnchor256), we shall discuss more on how we can perform one-click deployment using GitHub Actions and AWS Elastic Beanstalk.
