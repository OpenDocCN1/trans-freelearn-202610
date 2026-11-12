# # The Chief - BitBake

BitBake, the build engine, is a task scheduler (like GNU Make) which parses several scripts (shell and Python, for example).

Once the environment is built, BitBake will execute the task that has been requested. If no task is provided, BitBake will run the default task, called build.

To run a task, BitBake will first look for an environment variable called do_ <task_name>, which will contain the task code to execute (in Python or a shell). So, to compile a Yocto recipe, use the code contained in the do_compile variable.

In short, from the information contained in the recipes (or metadata), it downloads the sources of projects from the Internet, a local directory, or a version-control system (such as Git), and then builds in the order determined by the dependency graph generated dynamically. Finally, it installs binaries, generates the corresponding package, and builds the final image, which can be installed on the target (Raspberry Pi for us).

The following picture shows how BitBake works:

![](bbox=[0.005, 0.43, 0.815, 0.775])