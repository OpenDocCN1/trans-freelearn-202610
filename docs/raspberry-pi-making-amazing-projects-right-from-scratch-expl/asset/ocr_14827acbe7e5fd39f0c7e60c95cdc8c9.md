In this way, we can simulate an N-Body simulation with code running in parallel. Next, we will see how to install and get GalaxSee running.

> To learn more about the GalaxSee library, visit https://www.shodor.org/master/galaxsee/.

# Installing and running GalaxSee

Now that we have a basic idea of what an N-Body simulation is and how different bodies interact with each other, we will proceed to the installation of the GalaxSee library.

First, we need to download the archive of the source code. We can do this with the wget command:

```
wget http://www.shodor.org/refdesk/Resources/Tutorials/MPIExamples/Gal.tgz
```

Next, we extract the .tgz archive, assuming it was downloaded to the home folder. If the path is different, we need to enter the correct path:

```
tar -xzvf ~/Gal.tgz
```

Navigate to the folder:

```
cd Gal
```

We need to use Makefile to successfully build GalaxSee on our Raspberry Pi. To do that, open the file in the terminal:

```
nano Makefile
```

Then, change the line:

```
cc = mpicc
```

to:

```
cc = mpic++
```