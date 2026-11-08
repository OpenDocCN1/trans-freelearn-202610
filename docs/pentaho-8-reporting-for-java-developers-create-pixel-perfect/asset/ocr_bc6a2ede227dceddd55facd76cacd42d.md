# Building the release

Building the release is well described in the `README.md` file. All the required profiles are activated by the presence of a property named `release`. If you are going to build a macOS distribution as well, you must also specify the `mac.jre.path` property, as shown in the following command: **mvn clean install -Drelease -Dmac.jre.path=YOUR_MAC_OS_JRE_PATH** **(YOUR_MAC_OS_JRE_PATH example jre1.8.0_121.jre\Contents\Home)**

Once launched, the command will start to download a lot of dependencies, and a very long tail of log messages will appear. Once the subprojects are built, the unit tests will be launched, as well with everything that is requested to build the final package.

![](bbox=[0.143, 0.459, 0.207, 0.508])

> *I hope you will not have to complete the build in few minutes. Depending on your connectivity, any failures in downloading the dependencies and several other unexpected behaviors, you may need more than half an hour to complete this task.*

At the end of the execution, the final artifact will be generated in the `assemblies/winlinux(mac)/target` folder with the name `prd-ce-<version>.zip`. The ZIP file is everything you will need to run the Pentaho Report Designer, and also to build your personal projects based on Pentaho Reporting.