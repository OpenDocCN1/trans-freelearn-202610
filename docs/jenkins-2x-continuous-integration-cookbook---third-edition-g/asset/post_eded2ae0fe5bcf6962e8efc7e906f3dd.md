
## Getting ready

You need to have administrative access to Jenkins.

## How to do it...

1. Go to the Jenkins dashboard, then Manage Jenkins Prepare for Shutdown:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_166_0.png)

2. Go to the Jenkins dashboard again, then Manage Jenkins to Cancel Shutdown:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_166_1.png)

[Cancel Shutdown](https://)

## Monitoring Jenkins with JavaMelody

JavaMelody (http://code.google.com/p/javamelody/) is an open source project that provides comprehensive monitoring. The Jenkins plugin monitors both the master instance of Jenkins and also its nodes. The plugin provides a detailed wealth of important information. You can view evolution charts ranging from 1 day or 1 week to months for the main quantities, such as CPU or memory. Evolution charts are very good at pinpointing scheduled jobs that are resource-hungry.

The `Monitoring` plugin provides the monitoring of Jenkins with JavaMelody. It provides charts for CPU, memory, system load average, HTTP response time, and so on. It also provides details of HTTP sessions, errors and logs, actions for garbage collection, heap dumps, invalid session(s), and so on.

## Getting ready

Install the Monitoring plugin from the Jenkins dashboard:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_168_0.png)

## How to do it...

1. On the Jenkins dashboard, click on Manage Jenkins. Click on Monitoring of Jenkins master, as shown in the following screenshot:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_169_0.png)

2. It will open the statistics of JavaMelody monitoring, as shown in the following screenshot. Observe all of the statistics:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_169_1.png)

3. Click on Other charts and observe the other details related to Garbage Collector time,

## Threads

Threads on LAPTOP-FQ8JSR2E@192.168.99.1: Number = 42, Maximum = 51, Total started = 789 Details

| Thread | Daemon ? | Priority | State | Executed method | Cpu time (ms) | User time (ms) | Kill |
|---|---|---|---|---|---|---|---|
| Attach Listener | yes | 5 | RUNNABLE |  | 0 | 0 |  |
| AWT-Window | yes | 6 | RUNNABLE | sun.awt.windows.WToolkit.eventLoop(Native Method) | 93 | 31 |  |
| DestroyJavaVM | no | 5 | RUNNABLE |  | 3,515 | 2,203 |  |
| FilePath localPool [6d] | yes | 5 | TIMED_WAITING | sun.misc.Unsafe.park(Native Method) | 15 | 0 |  |
| Finalizer | yes | 8 | WAITING | java.lang.Object.wait(Native Method) | 328 | 109 |  |
| Handling GET monitoring from 0.0.0.0:0.1 : RequestHandlerThread[0x309] | yes | 5 | RUNNABLE | java.lang.Thread.sleep(Native Method) | 1,546 | 1,187 |  |
| [CHub]1 Selector (sun.nio.ch. gen 0) : Computer threadPoolForRemoting [9] | yes | 5 | RUNNABLE | sun.java2d.Windows.Selector$SubSelector.poll(Native Method) | 0 | 0 |  |
| Java2D Disposer | yes | 10 | WAITING | java.lang.Object.wait(Native Method) | 15 | 0 |  |
| javanotify | yes | 5 | TIMED_WAITING | java.lang.Object.wait(Native Method) | 140 | 15 |  |
| Jenkins cron thread | no | 5 | WAITING | java.lang.Object.wait(Native Method) | 0 | 0 |  |
| Jenkins UDP 33849 monitoring thread | no | 5 | RUNNABLE | sun.net.www.http.HttpClient.parseHeaders(Native Method) | 0 | 0 |  |
| jenkins.util.Timer [#10] | yes | 5 | TIMED_WAITING | sun.misc.Unsafe.park(Native Method) | 500 | 93 |  |
| jenkins.util.Timer [#1] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 359 | 125 |  |
| jenkins.util.Timer [#2] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 328 | 109 |  |
| jenkins.util.Timer [#3] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 578 | 218 |  |
| jenkins.util.Timer [#4] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 484 | 156 |  |
| jenkins.util.Timer [#5] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 328 | 156 |  |
| jenkins.util.Timer [#6] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 540 | 171 |  |
| jenkins.util.Timer [#7] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 593 | 205 |  |

Java Melody - Threads

## There's more...

Monitoring is the foundation of comprehensive testing and troubleshooting. This section explores the relationship between these issues and the measurements exposed in the plugin.

## Troubleshooting with JavaMelody - memory

Your Jenkins server can at times have memory issues due to greedy builds, leaky plugins, or some hidden complexity in the infrastructure. JavaMelody has a comprehensive range of memory measurements, including a heap dump and a memory histogram.

The Java virtual machine divides memory into various areas and, to clean up, it removes objects that have no references to other objects. Garbage collection can be CPU-intensive when it is busy, and the nearer to consumption of full memory, the busier garbage collection becomes. To an external monitoring agent, this looks like a CPU spike that is often difficult to track down. Just because the garbage collector manages memory, it is also a fallacy to believe that there is no potential for memory leakage in Java. Memory can be held for too long by many common practices such as custom caches or calls to native libraries.

Slow burning memory leaks will show up as gentle slopes on memory-related evolution graphs. If you suspect that you have a memory leak, then you can get the plugin to force a full garbage collection through the Execute the garbage collector link. If it is not a memory leak, the gentle slope will abruptly fall.

Memory issues can also express themselves as large CPU spikes as the garbage collector frantically tries to clean up, but can barely clean enough space. The garbage collector can also pause the application while comprehensively looking for no-longer referenced objects ("stop the world garbage collection"), thus causing long response times for web browser requests. This can be seen through the mean and max times in Statistics http - 1 day.

## Troubleshooting with JavaMelody - painful jobs

You should consider the following points:

- **Offload work:** For a stable infrastructure, offload as much work from the master instance as possible. If you have scheduled tasks, keep the heaviest ones separate in time. Time separation not only evens out load, but also makes finding the problematic build easier through the observation of the evolution charts of JavaMelody. Also consider spatial separation; if a given node or a labeled set of nodes shows problematic issues, then start switching around the machine location of jobs and view their individual performance characteristics through http://host:port/monitoring/nodes.
- **Hardware is cheap:** Compared to paying for human hours, buying an extra 8 GB is roughly equivalent to one man hour's effort.

A common gotcha is to add memory to the server, while forgetting to update the init scripts to allow Jenkins to use more memory.

- **Review the build scripts:** Javadoc generation and custom Ant scripts can fork JVMs and reserve memory defined within their own configuration. Programming errors can also be the cause of frustration. Don't forget to review JavaMelody's report on Statistic system error log and Statistic http system errors.
- **Don't forget external factors:** Factors include backups, cron jobs, updating the locate database, and network maintenance. These will show up as periodic patterns in the evolution charts.
- **Strength in numbers:** Use JavaMelody in combination with the disk usage plugin and others to keep a comprehensive overview of the vital statistics. Each plugin is simple to configure, but their usefulness to you will grow more quickly than the maintenance costs of adding extra plugins.

## Monitoring a Jenkins Job using a Build Monitor View

The `Build Monitor` plugin provides a visualization of the status and progress of selected Jenkins jobs. It displays an updated view automatically every couple of seconds using AJAX. It can easily accommodate different computer screen sizes too.
