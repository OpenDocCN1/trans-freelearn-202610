![](bbox=[0.11, 0.08, 0.85, 0.45])

With the memory dump copied to the REMnux machine, we can use a tool called the **Volatility Framework** to start searching for artifacts. The Volatility Framework (https://www.volatilityfoundation.org/) provides a collection of open source tools, implemented in Python, to help with the extraction of digital artifacts from volatile memory (RAM) images (samples). The framework supports memory images from all major 32- and 64-bit Windows, Linux, and Mac operating systems. Check their website for additional details.

The Volatility Framework comes preinstalled with the REMnux VM, there’s no need to add it. To get familiar with its functionality, let’s run through some of its commands.

See `https://www.howtoforge.com/tutorial/how-to-install-and-use-volatility-memory-forensic-tool/` for a tutorial on getting started with Volatility.