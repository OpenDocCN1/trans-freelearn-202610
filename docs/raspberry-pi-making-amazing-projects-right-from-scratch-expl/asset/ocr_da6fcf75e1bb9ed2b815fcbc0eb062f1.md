Save the file and reboot the Pi with sudo reboot. When it boots up, you should be auto-logged in.

We are now ready to proceed and begin installing the software that is required to run a cluster of computers.

# Installing the MPICH library

MPICH is a freely available, easily portable, and widely used implementation of the MPI standard, which is a message parsing protocol for distributed memory applications used in parallel computing. Work on the first version of MPICH was started around 1992 and is currently in its third version. MPICH is one of the most popular implementations of the MPI standard due to the fact that it is used as a foundation for a vast majority of MPI implementations, including ones by IBM, Cray, Microsoft, and so on.

> To know more about the MPICH library, visit http://www.mpich.org/.

At the time of writing, the latest version of MPICH is 3.2. The installation instructions for this are presented. However, these should work on all the sub-versions of MPICH3 and most of the later versions of MPICH2.

To begin the installation, ensure that the Raspberry Pi has a valid Internet connection. Run the following commands in order to get a working installation of MPICH:

+   1. Update your Raspbian distribution:
+      sudo apt-get update
+   2. Create a directory to store the MPICH files:
+      mkdir mpich
+      cd ~/mpich
+   3. Get the MPICH tar file:
+      wget http://www.mpich.org/static/downloads/3.2/mpich-3.2.tar.gz
+   4. Extract the tar archive:
+      tar xfz mpich-3.2.tar.gz