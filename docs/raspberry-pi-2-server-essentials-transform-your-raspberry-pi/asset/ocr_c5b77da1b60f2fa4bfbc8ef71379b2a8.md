So, to stop us from looking down into the console, I thought it would be a nice idea to see some information about our Raspberry Pi directly on the video stream.

With some help from Luca, I have created the C++ code that allows you to update the overlay pretty much on every frame using the UV4L API directly, which means hardly any CPU overhead is used.

If you feel comfortable with C++11, I have provided the complete code sample on the code download page, and you can add your own variables.

I have also provided a compiled binary with a variety of popular resource variables that you can use easily place in the template file once, and will be updated in real time on your stream.

# Using the overlay binary

The binary requires two parameters: the source video device and the source file, which will be referred to as the template file. The template file is exactly the same format as text.json, but the difference is that you construct the text fields with the inline variables listed here. It is recommended to copy your template file to /tmp/file.tpl and provide it as the source template.

You will also need to update the uv1_raspicam.config file to point to a new location, /tmp/text.json (not the template file), and restart the UV4L service. A new file will be created in /tmp/text.json from which the UV4L API call will read the replaced variables. Do not alter this file. Working with these files in /tmp is critical for achieving the best video performance but also eliminating I/O from your SD card, which may cause unnecessary wear during long periods of use.

![](bbox=[0.22, 0.58, 0.3, 0.64])

> In case you are wondering, you can update the template /tmp/file.tpl file while the binary is running. The C++ binary file reads the template file using a stream open in read-only mode with minimal locks.

Use this command line to execute the binary:

```
./overlay /dev/video0 /tmp/text.tpl
```

# Inline variables

Here are some useful variables from the general Raspberry Pi resources:

+   - {sysnfo.cputemp}