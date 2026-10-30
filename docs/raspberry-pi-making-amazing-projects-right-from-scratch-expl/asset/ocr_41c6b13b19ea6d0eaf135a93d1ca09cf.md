# 11
Build Your Own
Supercomputer with
Raspberry Pi

In the previous chapter, we learned how to get the Raspberry Pi to communicate with the Internet, along with some more topics. We learned how to set up the Twitter API and how to send out tweets using it. We also learned how to save data in a database and retrieve it later. Finally, we combined the two concepts and the weather station we built in Chapter 9, Grove Sensors and the Raspberry Pi to create a tweeting weather station that also saves the temperature and humidity data in a database.

In all the chapters prior to this one, we explored the Raspberry Pi functionalities. But this chapter will be different in the sense that we will learn how to combine two or more Raspberry Pi devices to create a more powerful computer. Indeed, if you combine enough Raspberry Pi devices, you can make your own supercomputer! Sounds interesting? We will cover the following in this chapter:

+   - Using a network switch to create a static network for Pi devices
+   - Installing and setting up MPICH2 and MPI4PY and running on a single node
+   - Using the static network of multiple Pi devices is to create an MPI (short for Message Parsing Interface) cluster
+   - Performance the benchmarking of cluster
+   - Running GalaxSee (N Body simulation)