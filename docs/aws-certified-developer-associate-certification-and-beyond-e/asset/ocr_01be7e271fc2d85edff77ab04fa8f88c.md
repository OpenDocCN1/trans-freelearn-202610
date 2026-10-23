```
sam-app --zsh - 80x24
iiaasademycreator@Iiass-MacBook-Air todolistamsam % ls
sam-app
iiaasademycreator@Iiass-MacBook-Air todolistamsam % cd sam-app
iiaasademycreator@Iiass-MacBook-Air sam-app % ls
README.md events samconfig.toml tests
__init__.py hello_world template.yaml
iiaasademycreator@Iiass-MacBook-Air sam-app %
```

![](bbox=[0.25, 0.27, 0.76, 0.29])

+   16. Next, you need to create another folder in the `todolistamsam` folder called `website`. This folder will be used to host the website application code for SAM to interact with.

+   17. Type in the `mkdir website` command and press *Enter*.

+   18. Next, using File Explorer (on Windows) or Finder (on Mac), navigate to the desktop and *unzip* the `index-01-06-224.zip` file you downloaded from the GitHub repository. (On a Mac computer, you simply double-click on the file to extract, and on Windows, you right-mouse-click on the file and select **Extract all**.) This will unzip the file to a folder that contains all the website application code. Copy the contents of this folder into the `website` folder you created in *step 17*. The following screenshot shows the contents of the folder in Finder on a Mac:

![](bbox=[0.22, 0.5, 0.76, 0.76])

![](bbox=[0.29, 0.77, 0.68, 0.79])

Note: You will also note in *Figure 14.10* that there are two important files created by the `sam init` command. The `template.yaml` file contains the template for the quick-start **Hello world application**. You will need to replace this template with the template you downloaded from the GitHub repository, which you updated earlier in *step 3*. You will also need to update the `samconfig.toml` file, which will be discussed shortly.