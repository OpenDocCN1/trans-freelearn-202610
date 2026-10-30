So, we have to install it manually. We do this by first cloning the repository from BitBucket:

```
git clone https://bitbucket.org/mpi4py/mpi4py
```

![](bbox=[0.185, 0.195, 0.815, 0.33])

Make sure that all the dependencies for the library are met. These are as follows:

+   - Python 2.6 or higher
+   - A functional MPICH installation
+   - Cython

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