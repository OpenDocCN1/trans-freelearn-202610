# # Configuring the Dart environment

This recipe will help customize the Dart environment according to our requirements. Here, we configure the following:

+   - Defining a `DART_SDK` environment variable
+   - Making `dart-sdk\bin` available for the execution of the Dart command-line tools

# ## Getting ready

We assume that you have a working Dart environment installed on your machine. If not, go to `https://www.dartlang.org/tools/download.html` and choose **Option 1** for your platform, which is the complete bundle. Downloading and uncompressing it will produce a folder named `dart`, which will contain everything you need. Put this in a directory of your choice. This could be anything, but for convenience keep it short, such as `d:\dart` on Windows or `~/dart` on Linux. On OS X, you can just drop the directory in the App folder.

# ## How to do it...

+   1. Create a `DART_SDK` environment variable that contains the path to the `dart-sdk` folder. On Windows, create and set `DART_SDK` to `d:\dart\dart-sdk` or `<your-dart-sdk-path>\dart-sdk` when using a dart from another folder (if you need more information on how to do this, refer to `http://www.c-sharpcorner.com/UploadFile/6cde20/use-of-environment-variable-in-windows-8/`). On Linux, add this to your configuration file `.bashrc` and/or `.profile` using the `export DART_SDK=~/dart/dart-sdk` code. On OS X, export `DART_SDK=/Applications/dart/dart-sdk` or in general `export DART_SDK=/path/to/dart-sdk`.
+   
+   2. The installation directory has a subfolder `dart-sdk\bin`, which contains the command-line tools. Add this subfolder to the path of your environment. On Windows, add `%DART_SDK%\bin` instead to the front of the path (system environment) variable and click on **OK**. On Linux or OS X, add `export PATH=$PATH:$DART_SDK/bin` to your configuration file.
+   
+   3. Reset your environment configuration file or reboot your machine afterwards for the changes to take effect.