
Let's dive further into GitHub, where we’ll learn how branching works and why branches are important. You and your friends are building a one-page portfolio website. You want to add new features without messing up what’s already there. That’s where GitHub Flow comes in.

GitHub Flow, in the context of branching, is a simple yet effective way to manage changes in a project, such as when building a one-page portfolio website.

## Setting up your own space – creating a new feature branch

To start using GitHub Flow, we need to create a new `feature branch`.

So, what’s a branch? Think of it as a copy of the website where you can experiment without affecting the main website (the `main` branch).

You decide to add a Skills section to the website. So, you create a new branch named `add-skills-section`. Now, you have a separate space to work on this feature.

### Understanding the main branch

The `main` branch is the final, public version of your website. Everything here is in good order, tested, and presentable.

Why not work directly on the `main` branch? Doing so is risky, like cooking a new dish for the first time in the middle of a busy kitchen. Mistakes can affect everyone.

### Deciding to add a new feature

Let’s say you want to add a Contact Me form to your website. It’s a new form style that you’ve never developed before.

You create a branch that comes off the `main` branch. This is your workspace. Name it something clear, such as `add-contact-form`.

Why use a `feature branch`?

If something goes wrong in your branch, the main website is unaffected. It’s like experimenting with your new contact form on a separate website. You can focus on your Contact Me form without worrying about messing up the main website. It’s your area to experiment, make mistakes, and learn. Once created, make sure to switch your working directory to your new branch.

Creating a `feature branch` in GitHub Flow is like setting up a workstation in a shared project. It gives you the freedom to experiment and develop your part of the project (such as your Contact Me form) without stepping on anyone else’s toes. Once your feature is ready and tested, it can be seamlessly integrated into the main project, enriching everything with your contribution.

## Making changes and committing

Figure 4.1 shows the process of creating a new `feature branch` in git. It visually represents how a `feature branch` is created from the `main` branch, allowing you to work on new features independently without affecting the main project. This helps in keeping the `main` branch stable while new features are being developed and tested. Once the new feature is ready and tested, it can be merged back into the `main` branch. This workflow ensures that the development process is organized and that the main project remains stable.

![Figure 4.1 – GitHub Flow – creating a new feature branch](image/B21847_04_1.jpg)
Figure 4.1 – GitHub Flow – creating a new feature branch

Once you’ve created your new `feature branch`es, you can start making some changes while adding new files to your `feature branch`.

You start adding elements for the Contact Me form. This could involve writing `HTML` for the layout, `CSS` for the style, or `JavaScript` for interactive features. Maybe you’ll lay out the basic structure, then add some style, and finally fine-tune the details.

In this personal workspace, you can add, edit, or delete files. Feel free to experiment here because your changes won’t impact the main project until you decide to merge them.

Once you’re happy with your changes, you `commit` them. This is like saying, “This part of my section is done for now.”

### What’s a commit?

Think of a `commit` as a checkpoint. Each `commit` represents a set of changes or additions you made.

Using Git, you’ll add the files you’ve changed to the staging area (with `git add`), and then you’ll create a `commit` with `git commit`. In this `commit`, you’ll write a message describing what you did – for example, Added basic layout for the blog section. It’s best to `commit` often because doing so allows you to see what changes you made and when. If something breaks or doesn't work as expected, it's easier to revert to a previous `commit` if you have smaller, more frequent `commit`s. Each `commit` is a snapshot of your work at a specific point in time. If something goes wrong later, you can always go back to a previous `commit`.

Every `commit` is recorded in the branch history. This means you can see the evolution of your Contact Me form over time, understanding what changes were made and when:

![Figure 4.2 – GitHub Flow – adding commits to a feature branch](image/B21847_04_2.jpg)
Figure 4.2 – GitHub Flow – adding commits to a feature branch

### Pushing changes

Once you’ve made a series of `commit`s that represent a significant portion of your work (such as completing the layout of the Contact Me form), you `push` these `commit`s to GitHub. This updates your branch on GitHub with all the changes you’ve made locally.

After pushing, your `commit`s are visible to your teammates. They can see exactly what changes you’ve made, `commit` by `commit`.

## Show and tell – creating a pull request

Once you’ve made your changes and you’re ready to show your Skills section to your friends, you can create a `pull request`. This is like putting your part of the mural on display for feedback. It’s like saying, “Hey, I’ve made some changes, can someone check them out?”

Why a `pull request` (`PR`)? This lets your team see what you’ve done, give suggestions, or say it’s ready to be part of the main website. This step is great for getting feedback from others on your team:

![Figure 4.3 – GitHub flow – Opening a PR](image/B21847_04_3.jpg)
Figure 4.3 – GitHub flow – Opening a PR

Opening a `PR` is like a mini-celebration of your work, combined with a brainstorming session. It’s where your effort becomes a team effort, bringing everyone’s talents together to make your one-page portfolio website the best it can be!

## Teamwork – discussing and reviewing the PR

Your friends look at your new Skills section. Say someone suggests Add more color here or Align this text better. They might give you tips on how to add some tests or which naming conventions to follow. It’s all about working together and learning from each other. Based on this feedback, you tweak your section. Every change you make is updated in the `PR`:

![Figure 4.4 – GitHub Flow – starting the code review](image/B21847_04_4.jpg)
Figure 4.4 – GitHub Flow – starting the code review

## Approving the PR

Once everyone is happy with the proposed changes and improvements and likes the Skills section, it’s time to finalize this and make it officially part of the website:

![Figure 4.5 – GitHub Flow – PR approval](image/B21847_04_5.jpg)
Figure 4.5 – GitHub Flow – PR approval

## Making it official – merging the PR

Once your changes have been reviewed and approved, you can merge the `PR`. This means your changes will be added to the main project. It’s like saying, “Okay, these changes are good to go!”

Now, your Skills section is ready to go live on the website:

![Figure 4.6 – GitHub Flow – merging the PR to the main branch](image/B21847_04_6.jpg)
Figure 4.6 – GitHub Flow – merging the PR to the main branch

## Cleanup time

After merging, you can delete the branch you worked on. It’s a bit like cleaning up your workspace after completing a project. But don’t worry – the history of your changes is still saved.
