Wireless info (the default is wlan0):

+   - `{signfo.level}`: Signal strength
+   - `{signfo.quality}`: Signal quality
+   - `{signfo.bitrate}`: Bitrate

Network bandwidth (the default is eth0):

+   - `{nnfo.downspeed}`
+   - `{nnfo.upspeed}`

A sample text line will look like this:

```
"ETH0 UP: {nnfo.upspeed} CPU temp {sysnfo.cputemp}"
```

To change the default values you need to amend the C++ file and recompile it. If you are already going to edit the code, use the base code as an example and try to add something else. If you have never edited C++ or any other code, just try to understand the main class line by line, and by the time you get to the end, you will understand the basic C++.

# Compiling the overlay code yourself

The C++ code was written in the C++11 standard, and it is necessary to install G++ version 4.8 to be able to compile this code. No extra libraries, such as libboost, are required for this sample:

```
sudo apt-get install g++-4.8
```

Once you have installed the latest G++, you can then simply compile the main code file and extra code files with any changes you have made by executing the following command:

```
sudo g++-4.8 -Ofast -std=c++11 *.cpp -o overlay
```

Luca has also provided a text-scrolling example that can be downloaded from http://www.linux-projects.org/downloads/examples/uv4l-overlay.cpp, but it requires the Boost C++ library. Unfortunately, the inclusion of this library causes the code to take considerably longer to compile. This is why I have removed all dependencies from the Boost library in my code; it provides a lot of extra support for tasks and structures used in modern programming languages and is worth a try:

```
sudo apt-get install libboost-all-dev
```