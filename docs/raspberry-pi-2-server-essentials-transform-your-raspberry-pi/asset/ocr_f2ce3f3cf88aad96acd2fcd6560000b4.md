# Installing DNVM, DNU and DNX

DNVM, DNU, and DNX can be installed using a one-line command on the terminal thanks to a script that will execute all the other required operations. Please note that the stable uses old command lines: K, KRE, KPM, and KVM. To stay up to date with patches, we will use the dev (bleeding edge) branch. These patches remove a lot of extra commands that will not be covered here. As soon as version 1 is released, please use the stable branch:

```
```bash
cd /home/pi
curl -sSL https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.sh | DNX_BRANCH=dev sh && source /home/pi/.dnx/dnvm/dnvm.sh
```
```

# Installing DNX

To install the latest version of DNX (.NET 5), which as of writing is Beta4, on the dev, we can now just type in the following one-line command:

```
```bash
dnvm upgrade
dnx
```
```

Congratulations, you now have .NET 5 installed on your Raspberry Pi!

# Running the MVC website

During the previous process, only the .NET 5 runtime environment was installed, which allowed for cross-platform execution of the .NET applications.

To get a quick MVC website running, we will use the examples from the ASPNET Github repository. During beta, please make sure you select the correct directory of the DNX version that was installed. In the rest of this chapter, we be using the v1.0.0-beta4 directories and DNX. When the final is released, it will be known as v1.0.0.

> Please double-check the DNX version and the sample version you are using. A mismatched DNX and sample will result in some of the samples failing to run.

We will clone the main APSNET Home Sample and change into the specific beta directory. We will run a package restore in this directory to install all the required dependencies, such as Kestrel: