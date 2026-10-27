
We get the following sign in screen:

![](img/8ededb1e0b69e85687f6e488d1044b79_57_0.png)

CDH Sign-up Screen

The terms and conditions are first laid out:

![](img/8ededb1e0b69e85687f6e488d1044b79_58_0.png)

The VM download for CDH starts:

![](img/8ededb1e0b69e85687f6e488d1044b79_58_1.png)

The CDH VM is more than 5 GB and can take a while to download.

Unzip the file. The folder will contain the files shown in the following image:

![](img/8ededb1e0b69e85687f6e488d1044b79_58_2.png)

Unzip downloaded file if it is in Zip format.

The download file is more than 5 GB and will take a bit of time depending on your Internet connection speed.

6. After the download completes, double-click on the .ova or .ovf file and it will open in Oracle VirtualBox:

    ![](img/8ededb1e0b69e85687f6e488d1044b79_59_0.png)

We get the following screen as a final step of the installation:

![](img/8ededb1e0b69e85687f6e488d1044b79_62_0.png)

This concludes the installation of the Hadoop environment using the Hortonworks Data Platform.

## Installing CDH in other environments

The CDH Quickstart VM can also be installed using VMware, Docker, and Cloud platforms. Instructions for the same are available at the links provided in the following pages.

## Installing Packt Data Science Box

We have also created a separate virtual machine for some of the exercises in the book.

Download the Packt Data Science Virtual Machine Vagrant files from

`https://gitlab.com/packt_public/vm.`

To load the VM, first download **Vagrant** from

`https://www.vagrantup.com/downloads.html.`

![](img/8ededb1e0b69e85687f6e488d1044b79_63_0.png)

Once you have completed the download, install **Vagrant** by running the downloaded Vagrant installation file. Once the installation completes, you'll get a prompt to restart the machine. Restart your system and then proceed to the next step of loading the vagrant file:

![](img/8ededb1e0b69e85687f6e488d1044b79_64_0.png)

Completing the Vagrant Installation

Click confirm on the final step to restart:

![](img/8ededb1e0b69e85687f6e488d1044b79_64_1.png)

Restarting System

In the terminal or command prompt, go to the directory where you have downloaded the Packt Data Science Vagrant files and run the following commands (shown in Windows):

```
$ vagrant box add packtdatascience packtdatascience.box

==> box: Box file was not detected as metadata. Adding it directly...

==> box: Adding box 'packtdatascience' (v0) for provider:

box: Unpacking necessary files from:
file://C:/Users/packt/Downloads/packt_public_vm/packtdatascience.box

box: Progress: 100% (Rate: 435M/s, Estimated time remaining: --:--:--)

==> box: Successfully added box 'packtdatascience' (v0) for 'virtualbox'!

$ vagrant box list

packtdatascience (virtualbox, 0)

C:\Users\Nataraj Dasgupta\Downloads\packt_public_vm>vagrant up

Bringing machine 'default' up with 'virtualbox' provider...

==> default: Importing base box 'packtdatascience'...

==> default: Matching MAC address for NAT networking...

==> default: Setting the name of the VM:
packt_public_vm_default_1513453154192_57570

==> default: Clearing any previously set network interfaces...

==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat

==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)

==> default: Booting VM...
...
```

If all goes well, you should see a new entry in Oracle VirtualBox:

![](img/8ededb1e0b69e85687f6e488d1044b79_66_0.png)

Double-click on the name of the box to start (and test) it. Log in with id/password as `packt/packt`:

```
Ubuntu 16.04.2 LTS vagrant tty1

vagrant login:

Ubuntu 16.04.2 LTS vagrant tty1

vagrant login: packt
Password:
Last login: Mon Aug 21 16:02:59 UTC 2017 on tty1
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-83-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

38 packages can be updated.
6 updates are security updates.

packt@vagrant:~$
```

## Installing Spark

The CDH Quickstart VM includes Spark as one of the components, and hence it will not be necessary to install Spark separately. We'll discuss more on Spark in the chapter dedicated to the subject.

Further, our tutorial on Spark will use the Databricks Community Edition which can be accessed from `https://community.cloud.databricks.com/`. Instructions on creating an account and executing the necessary steps have been provided in the Chapter 6, *Spark for Big Data Analytics*.

## Installing R

R is a statistical language that has become extremely popular over the last 3-5 years, especially as a platform that can be used for a wide variety of use cases, ranging from simple data mining to complex machine learning algorithms. According to an article posted in IEEE Spectrum in mid-2016, R takes the No. 5 spot among the Top 10 languages in the world.

Open source R can be downloaded from `https://www.r-project.org` via the CRAN site located at `https://cran.r-project.org/mirrors.html`.

Alternatively, you can download R from the Microsoft R Open page at `https://mran.microsoft.com/rro/`. This was earlier known as Revolution R Open, an enhanced version of open source R released by Revolution Analytics. After Microsoft acquired Revolution Analytics in 2015, it was rebranded under the new ownership.

Microsoft R Open includes all the functionalities of R, but also includes the following:

- Numerous R packages installed by default as well as a set of specialized packages released by Microsoft Corporation that complements the existing features of open source R
- Multi-threaded Math Libraries for multi-threaded computations in R
- A fixed CRAN Repository called MRAN. CRAN, the Comprehensive R Archive Network is a collection of sites that contains R binaries, packages, and associated content. Packages on CRAN can be updated on a continuous basis. MRAN takes a fixed snapshot of CRAN, which remains the same until the next release, thus allowing reproducibility and consistency.

### Steps for downloading and installing Microsoft R Open

We proceed as follows:

1. Go to `https://mran.microsoft.com` and click on the **Download Now** button:

    ![](img/8ededb1e0b69e85687f6e488d1044b79_68_0.png)

    Microsoft Open R Homepage

2. Select the distribution that is appropriate for your system (Windows, macOS, or Linux):

    ![](img/8ededb1e0b69e85687f6e488d1044b79_69_0.png)

    Microsoft Open R Versions

3. Once the download completes, double-click on the downloaded file to install Microsoft R Open.

4. Note that on a macOS, you may get an error message as follows:

    ![](img/8ededb1e0b69e85687f6e488d1044b79_70_0.png)

5. If this happens, right-click on the downloaded file and select **Open** from the menu that pops up. This will let you open the file manually and install:

    ![](img/8ededb1e0b69e85687f6e488d1044b79_70_1.png)

6. Once installed, double-click on **Microsoft R Open** to launch the application:

    ![](img/8ededb1e0b69e85687f6e488d1044b79_71_0.png)

### Installing RStudio

RStudio is an application released by `rstudio.org` that provides a powerful feature-rich graphical IDE (**integrated development environment**).

The following are the steps to install RStudio:

1. Go to `https://www.rstudio.com/products/rstudio/download`.

    ![](img/8ededb1e0b69e85687f6e488d1044b79_72_0.png)

2. Click on the link relevant for your operating system, download and install the respective file:

    ![](img/8ededb1e0b69e85687f6e488d1044b79_73_0.png)

    Downloading RStudio

3. Note that on a macOS, you may simply move the downloaded file to the Applications folder. On Windows and Linux operating systems, double-click on the downloaded file to complete the steps to install the file:

    ![](img/8ededb1e0b69e85687f6e488d1044b79_73_1.png)

    RStudio on the Mac (copy to Applications folder)

## Installing Python

We proceed with the installation as follows:

1. Similar to R, Python has gained popularity due to its versatile and diverse range of packages. Python is generally available as part of most modern Linux-based operating systems. For our exercises, we will use Anaconda from Continuum Analytics®, which enhances the base open source Python offering with many data-mining- and machine-learning-related packages that are installed natively as part of the platform. This alleviates the need for the practitioner to manually download and install packages. In that sense, it is conceptually similar in spirit to Microsoft R Open. Just as Microsoft R enhances the base open source R offering with additional functionality, Anaconda improves upon the offerings of base open source Python to provide new capabilities.
