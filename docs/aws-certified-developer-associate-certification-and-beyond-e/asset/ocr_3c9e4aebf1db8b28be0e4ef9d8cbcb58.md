# Project Task 14.1: Installing the AWS CLI and SAM CLI

To use SAM, you must install the AWS CLI and the SAM CLI onto your computer:

+   1. To install the AWS CLI tools, navigate to the AWS CLI web page at https://aws.amazon.com/cli/.
+   2. From the right-hand menu, download the installer that is appropriate for your operating system.
+   3. Once downloaded, launch the installer and follow the instructions to complete the installation of the CLI on your computer. Note that for Linux-based computers, you will need to follow the instructions on this page: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html#cliv2-linux-install.
+   4. To test that the installation completed successfully, launch Command Prompt in Windows or Terminal in macOS/Linux and run the aws --version command. You should get an output similar to the following screenshot:

![](bbox=[0.164, 0.333, 0.87, 0.397])

Figure 14.3 – AWS CLI version

+   5. Next, you must install the SAM CLI. Navigate to the following web page: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html.
+   6. You will find details on how to install the SAM CLI for your operating systems. You can also choose the GUI installers for Windows and Mac. For Mac computers, select the correct installer for your computer’s architecture, that is, x86_64 or ARM64 architecture.
+   For Linux-based computers, you will need to perform a command-line install as there is no GUI option. Again, ensure you follow the instructions for your computer’s architecture.
+   7. Once the installation has completed, launch Command Prompt if you are on Windows or Terminal if you are on a Mac or Linux computer and run the sam --version command.
+   8. You should get an output similar to the following screenshot, which would mean that the installation was successful:

![](bbox=[0.148, 0.666, 0.87, 0.732])

Figure 14.4 – AWS SAM version