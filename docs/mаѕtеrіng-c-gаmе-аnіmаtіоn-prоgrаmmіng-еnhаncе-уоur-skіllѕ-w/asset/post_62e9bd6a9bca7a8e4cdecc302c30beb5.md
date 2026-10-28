
As we will use open source software and platform-independent libraries in this book, you should be able to compile and run the code “out of the box” on Windows and Linux. You will find a detailed list of the required software and libraries, plus their installation, in the following *Technical requirements* section.

## Technical requirements

For this chapter, you will need the following:

- A PC with Windows or Linux, and the tools listed later in this section
- Git for source-code management
- A text editor (such as Notepad++ or Kate) or a full IDE (such as Visual Studio 2022 for Windows, or Eclipse/KDevelop for Linux)

> Important note
> 
> A recent C++ compiler is required to compile the code. In the current CMake build system, C++17 is configured, but the code is known to work with newer C++ standards, up to and including C++26 (although the compiler must support those standards).

Now, let’s get the source code for this book and start unpacking the code.

## Getting the source code and the basic tools

The code for this book is hosted on GitHub, which you can find here:

https://github.com/PacktPublishing/Mastering-Cpp-Game-Animation-Programming

You need to install Git since the build system utilizes Git to download the third-party projects used in the examples.

On Linux systems, use your package manager. For Ubuntu, the following line installs Git:

```
sudo apt install git
```

On Windows, you can download Git here: https://git-scm.com/downloads.

To unpack the code, you can use any of the following two methods.

### Getting the code using Git

To get the code in the book, you should use Git. Using Git offers you additional features, such as creating a local branch for your changes, keeping track of your progress, and comparing your updates to the example code. Also, you can easily revert changes if you have broken the code during the exploration of the source code, or while working on the practical sessions at the end of each chapter.

You can get a local checkout of the code in a specific location on your system either through the Git GUI, by cloning the repository in Visual Studio 2022, or by executing the following command in a CMD:

```
git clone https://github.com/PacktPublishing/Mastering-Cpp-Game-Animation-Programming
```

Please make sure that you use a path without spaces or special characters such as umlauts as this might confuse some compilers and development environments.

### Getting the code as a ZIP file

Although Git is recommended, you can also download the code as a ZIP file from GitHub. You will need to unpack the ZIP file to a location of your choice on your system. Also, make sure that the path the ZIP file is unpacked to contains no spaces or special characters.

Before we can use the code from the book, some tools and libraries must be installed. We will start with the Windows installation, followed by the Linux installation.

### Installing the required tools and libraries for Windows

To compile the example code on a Windows machine, I recommend using Visual Studio 2022 as the IDE since it contains all you need for a quick start. Using other IDEs like Eclipse, Rider, or KDevelop is no problem as the build is managed by CMake, but you may need to install a C++ compiler like MSYS2 plus the compiler packages as an additional dependency.

### Installing Visual Studio 2022 on Windows

If you want to use Visual Studio for the example files and don’t have it installed yet, download the free Community Edition of Visual Studio at https://visualstudio.microsoft.com/de/downloads/.

A double-click on the file will automatically start the Windows Registry Editor to import the settings to the Windows Registry. After confirming both the UAV dialog and the following warning dialogs by clicking Yes, the Registry Editor will import the new settings.

Now, reboot the PC to activate the long path names and continue with the installations.

## Downloading Open Asset Import Library

For Windows, Open Asset Import Library must be built and installed from the source files. Clone the repository from https://github.com/assimp/assimp in a new Visual Studio 2022 project, as shown in Figure 1.3:

![](img/3cfb9832c943f5f7366fd104b5255971_38_0.png)

As an alternative, you can create a clone from a Git Bash, or via the Git GUI:

```
git clone https://github.com/assimp/assimp
```

## Configuring the build

We need to make a few adjustments to create a static library instead of a dynamic library. Using a static library makes the build process easier for us, as we don't have to worry about an additional DLL file.

To change the CMake settings, choose the following option after right-clicking on the CMakeLists.txt file:

![](img/3cfb9832c943f5f7366fd104b5255971_38_1.png)

In the **Configuration** tab of Visual Studio 2022 that appears, change the configuration name to x64-RelWithDebInfo, and change the configuration type to RelWithDebInfo:

![](img/3cfb9832c943f5f7366fd104b5255971_39_0.png)

Figure 1.5: Modifying the current configuration of the asset importer

By using RelWithDebInfo, a release version with debug information will be created. The resulting executable will be optimized by the compiler, but the file still contains data to allow debugging the program in case of problems.

Next, change the following settings in the CMake settings. You can use the search field on the bottom left, named **Filter variables...**, to search for the specified setting:

- Disable building a shared library:

![](img/3cfb9832c943f5f7366fd104b5255971_39_1.png)

Figure 1.6: Switching the setting to create a static library

- Change the linking of the C runtime:

![](img/3cfb9832c943f5f7366fd104b5255971_39_2.png)

Figure 1.7: Linking the C runtime statically

- Remove the library suffix to create a file name without the compiler version:

![](img/3cfb9832c943f5f7366fd104b5255971_40_0.png)

Figure 1.8: Removing the suffix of the created file

Next, select Build and then Install in the context menu of the CMakeLists.txt file.

After the installation is finished, the following folder structure will be generated:

![](img/3cfb9832c943f5f7366fd104b5255971_40_1.png)

Figure 1.9: Asset importer library and includes

We have to make all the files discussed in this section available for all examples in the book. To do this, two options are available – copy the files to a fixed path or add an environment variable.

## Copying the Assimp files

First, create this folder on your computer:

```
C:\Program Files\assimp
```

Then, copy the two folders lib and include into it:

![](img/3cfb9832c943f5f7366fd104b5255971_40_2.png)

Figure 1.10: The two folders have been copied to the Program Files folder

## Compiling and starting the example code

Running the examples can be done in two different ways: following the book example by example or compiling all the code at once to browse all the examples.

Compiling the code can be done using the following steps:

1. To open an example project, choose Open a local folder from the Visual Studio 2022 start screen or Open CMake from the File menu of Visual Studio 2022, then navigate to the folder with the example code you want to compile, or to the top-level folder of the example code if you want to compile all examples at once. Visual Studio will automatically detect and configure CMake in the selected folder for you. The last line of the output window should be as follows:

```
1> CMake generation finished.
```

2. This confirms the successful run of the CMake file generation.

3. Now, set the startup item by right-clicking on the CMakeLists.txt file – step is required to build and run the project:

![](img/3cfb9832c943f5f7366fd104b5255971_42_0.png)

4. After setting the startup item, we can build the current project. Right-click on the CMakeLists.txt file and choose Build:

![](img/3cfb9832c943f5f7366fd104b5255971_42_1.png)
