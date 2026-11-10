# SUNG-TAEK KIM

Sung-Taek Kim is a big-data engineer, enthusiastic about solving problems in funny and unique ways. He is currently focused on building a great developer experience.
magpi.cc/2dTJHUR

![](bbox=[0.58, 0.08, 0.68, 0.15])

Six Raspberry Pi boards are housed together inside the enclosure

An Ethernet router is used to connect the devices together

One of the Raspberry Pi units is the master; it controls the other Pi boards

![](bbox=[0.0, 0.09, 1.0, 0.49])

# PI SPARK BIG DATA CLUSTER

One data scientist shows us how to string six Raspberry Pis together to build a supercomputer and experiment with big data

# Quick Facts

+   - Pis aside, the parts only cost around $60
+   - 106 screws are needed for the enclosure
+   - A cluster of Raspberry Pis is also known as a 'Bramble'
+   - GHCQ has the largest Pi Bramble: 66 boards
+   - China's Tianhe-2 is the biggest supercomputer: 3,120,000 cores

The Raspberry Pi is great for learning computer science, but there’s one area that’s big news but requires big computers, and that’s ‘big data’. Big data software typically runs on clusters of networked computers, working together to perform the heavy lifting required. This clustered nature makes learning big data tricky, because you need several computers wired together to practise. Sung-Taek Kim, a software engineer from Korea, decided that the Raspberry Pi would be perfectly suited to the task. “Raspberry Pi is a great education platform to learn how big data software works,” he tells us. “It is [comparatively] slow and low-powered, [so] that you would have hands-on experiences when your data manipulation methods execute as planned.”

In fact, the light performance of the Raspberry Pi becomes an advantage when learning big data techniques. “Once you miss a small detail,” explains Sung-Taek, “you feel the operation processes slow down.” “Sending data across [a] network takes time,” he adds. “All the CPUs in your cluster compete for resources such as memory or disk [space], and a node or two could suddenly refuse to work, just like [in] a Google-class data centre cluster.” He explains that the relative slowness of a Pi cluster is actually an advantage, enabling you to prepare for such events.

Sung-Taek’s cluster is based around six Raspberry Pi 2 boards wired together with Ethernet cables via a D-Link 8-port Gigabit Desktop Switch. “Theoretically, you would only need one Raspberry Pi,” says Sung-Taek, “since Spark exploits the [nature] of a master-slave