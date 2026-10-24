+   Inside the textbox of the shell command, we enter the `printenv` command to be executed during the execution of the job pipeline, as illustrated in the following screenshot:

![](bbox=[0.1367, 0.166, 0.8652, 0.4121])

Figure 7.16 – Jenkins job command step sample

+   4. Then, we finish the configuration by clicking on **Apply** and then on the **Save** button.

Our Jenkins CI job has now been created and is configured to be triggered during a commit and to perform various actions.

We will now run it manually to test its proper functioning.

# # Executing a Jenkins job

To test job execution, we will perform these steps:

+   1. First, we will modify the code of our GitHub repository—for example, by modifying the `Readme.md` file.

+   2. Then, we commit to the master branch directly from the GitHub web interface.

+   3. What we see in Jenkins, right after making this commit, is that the `DemoCI` job is queued up and running.