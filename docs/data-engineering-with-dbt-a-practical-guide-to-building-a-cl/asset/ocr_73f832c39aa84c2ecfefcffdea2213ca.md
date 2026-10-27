+   2. Create a new feature branch to work on code by clicking the **Create branch** button and providing a name for the branch in the popup that appears.

+   3. After a few seconds, you will find yourself in the new branch (in the following screenshot, named **chapter08**) that will not be read-only anymore.

![](bbox=[0.33, 0.2, 0.69, 0.39])

Figure 11.2: Available Git actions in a clean state development branch

When a branch has no changes still to be committed, it is in a clean state, as the working directory is in sync with the Git repo, and you have the option to create a new PR.

+   4. You can now edit and develop the feature as required.

![](bbox=[0.33, 0.52, 0.69, 0.71])

Figure 11.3: The available Git actions in a dirty state branch

Once you edit one file, your branch will be in a dirty state, as the working directory is not in sync with the Git repo. Changed files will be listed in the **Version Control** area, and you can click on the filenames there to see what the changes are, with respect to the previous version.

If you want to abandon all the changes since the last commit and go back to the state as of the last commit of the branch, you can click **Revert** and confirm.