6. You should see the following screen when you connect to the instance:

Session ID: root-7rtyphzgodxtqmx67gsdvppjce Instance ID: i-0ff02a553fb1b10af

![](bbox=[0.13, 0.18, 0.85, 0.24])

Figure 11.14: Using Session Manager to connect to your instance

7. Next, you must install the Docker engine on this server:

+   I. Type the sudo yum update -y command and press Enter. This will perform any pending updates to the OS. In most cases, no updates will be required. The use of sudo will elevate your privileges to run these commands.

+   II. Next, type in the sudo yum install docker command and press Enter. This will state the installation of the Docker package. You will be asked to confirm whether you want to download the packages and install them. Go ahead and type in y and press Enter.

+   III. Once the download and installation are successful, you will get the Complete! message, as shown in Figure 11.15:

![](bbox=[0.13, 0.5, 0.85, 0.62])

Figure 11.15: Successfully installed Docker

+   IV. Next, type in the clear command and press Enter. This should clear the screen so it's not too cluttered, making it easier to read.