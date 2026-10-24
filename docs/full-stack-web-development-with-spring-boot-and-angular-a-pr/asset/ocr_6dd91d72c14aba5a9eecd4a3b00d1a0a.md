The command will successfully install the manager in your terminal. To check whether the installation is successful, we can execute the following command:

```
sdk version
```

This will display the current SDKMAN version installed on your machine.

# Installation on Windows

We need several steps to install SDKMAN on Windows since SDKMAN requires Bash tools. In this case, the first thing we need is a Git Bash Environment (MinGW):

+   1. First, we will install 7-Zip, and after successful installation, we will execute the following command to make a symbolic link in the Git Bash environment, having 7-Zip as the zip command:

```
ln -s /c/Program\ Files/7-Zip/7z.exe /c/Program\ Files/Git/mingw64/bin/zip.exe
```

Now we can use the commands we use in Linux to install the SDKMAN.

+   2. To install SDKMAN, we will execute the following command:

```
export SDKMAN_DIR="/c/sdkman" && curl -s "https://get.sdkman.io" | bash
```

+   3. After the installation, restart the Git Bash shell to run the SDKMAN commands. We can execute the following command to verify that our installation is successful:

```
sdk version
```

It will prompt the current version of SDKMAN installed in our terminal.

We are done installing and configuring SDKMAN on our machine on different operating systems. In the next section, we will configure IntelliJ IDEA with the installed Java 17.

# Setting up IntelliJ IDEA with Java 17

We have installed our IDE (IntelliJ IDEA) and Java 17 on our machine in the previous sections. Now, we will guide you on how to configure Java 17 on new and existing projects.