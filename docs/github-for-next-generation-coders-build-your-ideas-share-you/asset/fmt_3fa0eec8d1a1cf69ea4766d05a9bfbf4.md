
And that’s it! By following these simple steps, you’ve learned how to use GitHub to build something amazing with your friends. Using GitHub Flow, you and your friends can work on different parts of your one-page portfolio website simultaneously and efficiently. Each branch is a playground for new ideas, and PRs are where those ideas get polished and readied for the world to see. It’s a smooth, collaborative process that helps keep your project organized and moving forward!

# Understanding GitHub Flow in practice – two developers, two features, one project

Let’s explore how GitHub Flow works through an example with two developers, Alex and Jamie, working on a project to improve a website. They are each responsible for different features, and they use GitHub Flow to manage their contributions seamlessly.

The `main` branch is always a starting point for creating a feature branch. This is the stable version of the project, like the foundation of a building. Alex and Jamie will both start their work from here.

*Let’s try to explain this step **by step.*

Alex needs to add a contact form. They create a branch called `feature/contact-form` from the `main` branch. This branch is their workspace.

While Alex was working on the new feature branch they created previously, Jamie started on a gallery feature in a branch called `feature/gallery`:

![Figure 4.7 – GitHub Flow – branch creation](image/B21847_04_7.jpg) 

Figure 4.7 – GitHub Flow – branch creation

Both Alex and Jamie are coding their features, regularly committing their changes to their feature branches. These commits act like checkpoints in their work:

![Figure 4.8 – GitHub Flow – adding and committing files](image/B21847_04_8.jpg) 

Figure 4.8 – GitHub Flow – adding and committing files

Once the contact form works well, Alex pushes their branch to GitHub. They create a PR on GitHub, asking to merge `feature/contact-form` into the `main` branch. This is like saying, “*Hey team, I think this is ready to be part of our **main project.*”

The team reviews Alex’s work. Once it’s approved, the PR is merged into the `main` branch. The `feature/contact-form` branch is deleted to clean up the branches. The `main` branch now contains the new contact form feature:

![Figure 4.9 – GitHub Flow – pushing the branch, creating a PR on GitHub, and merging the PR](image/B21847_04_9.jpg) 

Figure 4.9 – GitHub Flow – pushing the branch, creating a PR on GitHub, and merging the PR

After Alex merges their contact form, Jamie pulls the latest `main` branch to update `feature/gallery`. This step ensures Jamie’s branch is up to date, reducing the risk of conflicts. Jamie keeps working on the gallery, making commits to their branch:

![Figure 4.10 – GitHub Flow – syncing the local repository with the newest main branch](image/B21847_04_10.jpg) 

Figure 4.10 – GitHub Flow – syncing the local repository with the newest main branch

Once satisfied, Jamie pushes their branch to GitHub and opens a PR for merging into the `main` branch. The team reviews Jamie’s gallery feature. They might suggest changes or improvements. After any final adjustments and team approval, Jamie’s gallery feature is merged into the `main` branch:

![Figure 4.11 – GitHub Flow – creating a PR on GitHub and merging it](image/B21847_04_11.jpg) 

Figure 4.11 – GitHub Flow – creating a PR on GitHub and merging it

And what’s the result of this type of flow? A collaboratively enhanced project.

The `main` branch now contains both Alex’s contact form and Jamie’s gallery as they have been seamlessly integrated. By regularly updating their feature branches with the latest `main` branch, Alex and Jamie avoided merge conflicts, making the integration smoother. The use of PRs allowed for team input and review, ensuring high-quality additions to the project.

Through GitHub Flow, Alex and Jamie were able to work independently on their features while ensuring their work aligned with the evolving project. This approach encourages collaboration, maintains project stability, and ensures new features are integrated smoothly:

![Figure 4.12 – GitHub Flow – actual main with all feature branches](image/B21847_04_12.jpg) 

Figure 4.12 – GitHub Flow – actual main with all feature branches

You’ve now seen how GitHub Flow works in practice with two developers working on different features and keeping the project organized. This process helps maintain a smooth workflow and ensures that new features are integrated without disrupting the main project.

Next, let’s learn how to create a local copy of your repository by cloning it. This step is essential for working on your projects locally on your computer. We’ll walk through the process of cloning a repository so you can start making changes and contributing to your project right away.

# Creating a local copy of the repository (cloning)

If you still have not cloned your repository, **My First Repo**, from GitHub, now would be the time to do so. In [*Chapter 2*](#B21847_02.xhtml#_idTextAnchor045), we explained how to clone the repository from GitHub, but let’s quickly recap it here.

You will be working on a one-page portfolio website and you need to add a left-hand side menu, a profile picture, social icons, and some CSS styling. Let’s go through the steps, starting from cloning the project to adding and committing your changes.

## Cloning the project

Follow these steps:

1.  **Navigate to the Code tab on GitHub**: Find your project’s repository on GitHub:

![Figure 4.13 – GitHub repository](image/B21847_04_13.jpg) 

Figure 4.13 – GitHub repository

1.  **Clone the repository**: Click the **Clone** or **Download** button and copy the URL provided:

![Figure 4.14 – GitHub repository clone URL](image/B21847_04_14.jpg) 

Figure 4.14 – GitHub repository clone URL

1.  **Open Git Bash or use your Terminal**: On your computer, open your Git Bash application or your terminal.

2.  **Run git clone**: Type `git clone <CLONE-URL>`, replacing `<CLONE-URL>` with the URL you copied. This command downloads a full copy of the repository to your local machine:

![Figure 4.15 – Running the git clone command](image/B21847_04_15.jpg) 

Figure 4.15 – Running the git clone command

1.  **Change directory**: Enter `cd <REPOSITORY-NAME>`, substituting `<REPOSITORY-NAME>` with the name of the repository you just cloned. This will move you into the repository’s directory:

![Figure 4.16 – Entering your new repository folder](image/B21847_04_16.jpg) 

Figure 4.16 – Entering your new repository folder

Now that we have cloned the project, let’s switch to the correct branch.

## Switching to the correct branch

Follow these steps:

1.  **List all branches**: Type `git branch -a` to see all available branches, both local and remote:

![Figure 4.17 – Running the git branch -a command to list all branches](image/B21847_04_17.jpg) 

Figure 4.17 – Running the git branch -a command to list all branches

1.  **Switch branch**: If you need to work on a specific branch, use `git switch <BRANCH-NAME>`, replacing `<BRANCH-NAME>` with the name of the branch you want to switch to. If you need to create a new branch, then use `git switch -c <BRANCH-NAME>`:

![Figure 4.18 – Running the git switch command to create a new branch and switch to it](image/B21847_04_18.jpg) 

Figure 4.18 – Running the git switch command to create a new branch and switch to it

Now that you’ve successfully cloned a project from GitHub and switched to the correct branch, you are ready to start adding your new files, various sections of the website, and different bits of styling. In the next section, you’ll start building your one-page portfolio website.

# Adding and committing files to a local repository
