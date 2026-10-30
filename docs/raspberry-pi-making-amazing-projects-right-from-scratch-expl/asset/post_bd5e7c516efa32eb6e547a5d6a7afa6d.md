
This imports the dependencies required for the speech output. Using espeak from Python is also as easy as:

```
>>> espeak.synth("I am the weatherman")
```

The synth method in the espeak module accepts a string and produces speech based on that. It's not the best-sounding voice, but it works!

To add this to the weather station, we can add the following statement inside our while loop:

```
espeak.synth("The current temperature is : " + str(temp))
```

```
espeak.synth("Humidity level is : " + str(hum) + "percent")
```

This works because Python supports string concatenation. This means that we can combine two strings just by adding a + sign between them. However, the temp and hum variables are not string variables. So we need to convert them to strings by the str() method. The str() method takes any type of variable and converts it into a string.

We can also use the espeak library to tell us many different things about our environment by getting the state from the Grove Sensors.

## Summary

In this chapter, we learned how to use Python to communicate with Twitter by using an open source library, Tweepy. We learned how to use Tweepy to send tweets. We also learned how to create a database, save and update data in it, and get some data from an existing database. Toward the end, we extended the capabilities of the weather station we built in Chapter 9, Grove Sensors and the Raspberry Pi and added tweeting and data-saving functionalities to it. We also added speech capabilities to it to make it easier to use.

In the next chapter, we will be exploring the interesting topic of parallel computing. We will be using multiple Raspberry Pis to create a cluster of computers that perform computations faster by working in tandem towards a single task. A master Pi will control other slave Pis and outsource its computations to them, so that the task is performed faster. We will use this setup to perform an n-body simulation, which is by itself a very computationally challenging task.

## 11
### Build Your Own
### Supercomputer with
### Raspberry Pi

In the previous chapter, we learned how to get the Raspberry Pi to communicate with the Internet, along with some more topics. We learned how to set up the Twitter API and how to send out tweets using it. We also learned how to save data in a database and retrieve it later. Finally, we combined the two concepts and the weather station we built in Chapter 9, Grove Sensors and the Raspberry Pi to create a tweeting weather station that also saves the temperature and humidity data in a database.

In all the chapters prior to this one, we explored the Raspberry Pi functionalities. But this chapter will be different in the sense that we will learn how to combine two or more Raspberry Pi devices to create a more powerful computer. Indeed, if you combine enough Raspberry Pi devices, you can make your own supercomputer! Sounds interesting? We will cover the following in this chapter:

- Using a network switch to create a static network for Pi devices
- Installing and setting up MPICH2 and MPI4PY and running on a single node
- Using the static network of multiple Pi devices is to create an MPI (short for Message Passing Interface) cluster
- Performance the benchmarking of cluster
- Running GalaxSee (N Body simulation)

## Introducing a Pi-based supercomputer

The basic requirements for the building of a supercomputer are as follows:

- Multiple processors
- A mechanism to interconnect them

We already have multiple processors in the form of multiple Raspberry Pi devices. We connect them via a networking hub, which is connected to each Pi via an Ethernet cable. A networking hub is nothing but a router that can accept Ethernet cables. There are dedicated networking hubs that can support many connections but for the purposes of this chapter, we can use a normal Wi-Fi router that accepts four LAN connections in addition to wireless connections. This is because we will not build a cluster of more than four Pi devices. You are welcome to add more devices to this cluster in order to increase your computing power. So, the hardware requirements for this chapter are as follows:

- Three Raspberry Pi devices
- Three Raspbian-installed SD cards
- A networking hub
- Three Ethernet cables
- Power supply for every device

## Installing and configuring MPICH2 and MPI4PY

Before we can begin installing the libraries to a network of multiple Pi devices, we need to configure our Raspbian installation to make things a bit easier.

Boot up a Raspberry Pi and in the terminal, enter the following command:

```
sudo raspi-config
```

3. Now, enter the advanced menu by selecting **Advanced Options**. We need to configure the following:

- 1. Set the hostname to `Pi1`.

![](img/631e8b200cce187082ee46b0eb1681b3_200_0.png)

- 2. Enable SSH.

![](img/631e8b200cce187082ee46b0eb1681b3_200_1.png)

We need to enable auto login so that we do not need to manually log in every Pi once we fire it up. Auto login is enabled by default in the latest versions of the Raspbian operating system, so we don't need to perform the following procedure on the latest version. To enable auto login, exit the configuration menu, but don't reboot yet. To set the auto login, open the inittab file with the following command:

```
sudo nano /etc/inittab
```

Comment out the following line:

```
1:2345:respawn:/sbin/getty --noclear 38400 tty1
```

This is done so that it looks like this:

```
#1:2345:respawn:/sbin/getty --noclear 38400 tty1
```

Then, add the following line:

```
1:2345:respawn:/bin/login -f pi tty1 </dev/tty1 >/dev/tty1 2>&1
```

![](img/631e8b200cce187082ee46b0eb1681b3_201_0.png)

Save the file and reboot the Pi with sudo reboot. When it boots up, you should be auto-logged in.

We are now ready to proceed and begin installing the software that is required to run a cluster of computers.

## Installing the MPICH library

MPICH is a freely available, easily portable, and widely used implementation of the MPI standard, which is a message passing protocol for distributed memory applications used in parallel computing. Work on the first version of MPICH was started around 1992 and is currently in its third version. MPICH is one of the most popular implementations of the MPI standard due to the fact that it is used as a foundation for a vast majority of MPI implementations, including ones by IBM, Cray, Microsoft, and so on.

> To know more about the MPICH library, visit http://www.mpich.org/.

At the time of writing, the latest version of MPICH is 3.2. The installation instructions for this are presented. However, these should work on all the sub-versions of MPICH3 and most of the later versions of MPICH2.

To begin the installation, ensure that the Raspberry Pi has a valid Internet connection. Run the following commands in order to get a working installation of MPICH:

- 1. Update your Raspbian distribution:
  sudo apt-get update
- 2. Create a directory to store the MPICH files:
  mkdir mpich
  cd ~/mpich
- 3. Get the MPICH tar file:
  wget http://www.mpich.org/static/downloads/3.2/mpich-3.2.tar.gz
- 4. Extract the tar archive:
  tar xfz mpich-3.2.tar.gz

5. This will contain the MPICH installation and build the following:

```
sudo mkdir /home/rpimpi/
sudo mkdir /home/rpimpi/mpi-install
mkdir /home/pi/mpi-build
cd /home/pi/mpi-build
```

6. Fortran is a dependency for MPI:

```
sudo apt-get install gfortran
```

Now, since we created a build folder for the MPICH installation earlier, we need to configure the installation script and point it to the folder where we want the interface to be installed. We do this by running the configure script and setting the build prefix to the installation directory:

```
sudo /home/pi/mpich/mpich-3.2/configure -prefix=/home/rpimpi/mpi-install
```

Finally, we carry out the actual installation.

```
sudo make
```

```
sudo make install
```

> Keep in mind that this build might take about an hour to complete. Make sure that the Raspberry Pi stays on for the entire period of the build.

Edit the bashrc file so that the path to the MPICH installation is loaded to the PATH variable every time we open a new terminal window. To do that, open the file in the terminal with the following command:

```
nano ~/.bashrc
```

Then, add the following line at the end:

```
PATH=$PATH:/home/rpimpi/mpi-install/bin
```

Save and exit the bashrc file by pressing Ctrl + X and then click on Return. We will now test the installation. For that, reboot the Pi:

```
sudo reboot
```
