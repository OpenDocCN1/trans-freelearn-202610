
Then, run the following command:

```
mpiexec -n 1 hostname
```

If the preceding command returns Pi1, or whatever you set your hostname to be, then our MPICH installation is successful and we can proceed to the next step.

We will now look at a short C program that uses MPICH. This is given as follows:

```
#include "mpi.h"
#include <stdio.h>

void main(int argc, char *argv[]) {
    int rank, size;
    MPI_Init(&argc, &argv);
    MPI_Comm_rank( MPI_COMM_WORLD, &rank);
    MPI_Comm_size( MPI_COMM_WORLD, &size);
    printf("I have %d rank and %d size \n", rank, size);
    MPI_Finalize();
}
```

This program runs on a single node and discovers the total number of nodes in a network. It then communicates with the MPI processes on other nodes and finds out its own rank.

The MPI_Init() function initiates the MPICH process on our computer according to the given information about the IP addresses in a network. The MPI_Comm_rank() function and the MPI_Comm_size() function get the rank of the current process and the total size of the network and save them in rank and size variables, respectively. The MPI_Finalize() function is nothing but an exit routine that ends the MPI process cleanly.

On its own, MPICH can be run using C and Fortran languages. But that might be a hindrance for a lot of users. But fret not! We can use MPICH in Python by installing the Python API called mpi4py.

## Installing mpi4py

The conventional way of installing the mpi4py module from a package manager such as apt-get or aptitude does not work because it tries to install the openMPI library along with it, which conflicts with the existing installation of MPICH. So, we have to install it manually. We do this by first cloning the repository from BitBucket:

```
git clone https://bitbucket.org/mpi4py/mpi4py
```

![](img/631e8b200cce187082ee46b0eb1681b3_205_0.png)

Make sure that all the dependencies for the library are met. These are as follows:

- Python 2.6 or higher
- A functional MPICH installation
- Cython

The first two dependencies are already met. We can install Cython with the following command:

```
sudo apt-get install cython
```

The following commands will install mpi4py with the setup.py installation script:

```
cd mpi4py
python setup.py build
python setup.py install
```

Add the path to the installation of mpi4py to PYTHONPATH so that the Python environment knows where the installation files are located. This ensures that we are easily able to import mpi4py into our Python applications:

```
export PYTHONPATH=/home/pi/mpi4py
```

Finally, run a test script to test the installation:

```
python demo/helloworld.py
```

Now that we have completely set up the system to use MPICH, we need to do this for every Pi we wish to use. We can run all of the earlier commands in freshly installed SD cards, but the better option would be to copy the card we prepared earlier into blank SD cards. This saves us the hassle of completely setting up the system from scratch. But remember to change the hostname to our standard scheme (such as Pi01 and Pi02) after the fresh SD card has been set up in a new Raspberry Pi. For Windows, there is an easy-to-use tool called Win32 Disk Imager, which we can use to clone the card into an .img file and then copy it to another blank SD card. If you are using OS X or Linux, then the following procedure should be followed:

- 1. Copy the OS from an existing card:

```
sudo dd bs=4M if=/dev/disk2 of=~/Desktop/raspi.img
```

- 2. Remove the card and insert a new card.
- 3. Unmount the card.
- 4. Format the SD card.
- 5. Copy the OS to the SD card:

```
sudo dd bs=4M if=~/Desktop/raspi.img of=/dev/disk2
```

> Replace disk2 with your own SD card

> To know more about the mpi4py library and to have a look at the various demos, you can visit the official Git repository on BitBucket at https://bitbucket.org/mpi4py/mpi4py.

## Setting up the Raspberry Pi cluster

Now that we have a running Raspbian distribution in all the Raspberry Pi devices that we wish to use, we will now connect them to create a cluster. We would need a networking hub that has at least the same number of LAN ports as the size of your cluster should be. For the purposes of this chapter, we will use three Raspberry Pis, so we would need a hub that has at least three LAN ports.

If you have a Wi-Fi router, then it should already have some ports at the back, as shown in the following image:

![](img/631e8b200cce187082ee46b0eb1681b3_207_0.png)

We also need three networking cables, such as those with an Ethernet connector, where one port goes into the Raspberry Pi and the other goes into the networking hub. They will look like the ones shown in the following image:

![](img/631e8b200cce187082ee46b0eb1681b3_208_0.png)

Connect one end of the Ethernet cable to the Raspberry Pi and the other end to the router for all the devices. Once we complete this networking setup, we can move on to connecting the Raspberry Pi devices with software.

## Setting up SSH access from the host to the client

In the setup described here, one master node will control the other slave nodes. The master is called the host and the slaves are called clients. To access the client from the host, we use something called SSH. It is used to get the terminal access to the client from the host, and the command used is as follows:

```
ssh pi@192.168.1.5
```

Here, the preceding IP address can be replaced with the IP address of our Raspberry Pi. The IP address of a Pi can be found by simply opening up a new terminal and entering the following command:

```
ifconfig
```

This will give you the IP address associated with your connected network and the interface, which is eth0 in our case. A small problem with using this is that every time we try to SSH into a client, we need a password. To remove this restriction, we need to authorize the master to log in to the client. How we do that is by generating an RSA key from the master and then transferring it to the client. Each device has a unique RSA key, and hence, whenever a client receives an SSH request from an authorized RSA device, it will not ask for the password. To do that, we must first generate a public private key pair from the master, which is done using a command in the following format:

```
ssh-keygen -t rsa -C "your_email@youremail.com"
```

It will look something like this:

![](img/631e8b200cce187082ee46b0eb1681b3_210_0.png)

To transfer this key into the slave Raspberry Pis, we need to know each of their IP addresses. This is easily done by logging into their Pi and entering ifconfig.

Next, we need to copy our keys to the slave Raspberry Pi. This is done by the following command, replacing the given IP address with the IP address of your own Raspberry Pi:

```
cat ~/.ssh/id_rsa.pub | ssh pi@192.168.1.5 "mkdir .ssh;cat >> .ssh/authorized_keys"
```

Now that we have copied our keys to the slave Raspberry Pi, we should be able to log in without a password with the standard SSH command:

```
ssh pi@192.168.1.5
```

## Running code in parallel

We have now successfully completed the prerequisites for the running of code on multiple machines. We will now learn how to run our code on the Raspberry Pi cluster using the MPICH library. We will first run a demo included with the `mpi4py` module, and then we will run an N-Body simulation on our cluster. Sounds fun? Let's get into it.

First, we need to tell the MPICH library the IP address of each of the Pis in our network. This is done by simply creating a new file called `machinefile` in the home folder, which contains a list of the IP addresses of all the Raspberry Pis connected to our network. Execute the following command to create the file:

```
nano machinefile
```

Then, add the IP address of each Raspberry Pi in a new line so that the final result looks like this:

![](img/631e8b200cce187082ee46b0eb1681b3_211_0.png)

Note that we have added three IP addresses because two Raspberry Pis are slaves and one is a master. So, the MPICH library knows that it needs to run the code on these three devices. Also, note that MPICH does not differentiate between devices. It just needs a valid installation of the library to run properly. This means that we can add different versions of Raspberry Pi to our cluster or even other computers such as a BeagleBone, which has a valid installation of MPICH.

To test our setup, we navigate to the `mpi4py/demo` directory with the following command:

```
cd ~/mpi4py/demo
```
