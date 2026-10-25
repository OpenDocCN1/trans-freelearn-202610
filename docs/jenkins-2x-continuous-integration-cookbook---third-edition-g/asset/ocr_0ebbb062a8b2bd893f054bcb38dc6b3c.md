# How to do it...

Let's see how to modify the Jenkins configuration.

+   1. In the top-level directory of Jenkins, look for the `config.xml` file. Edit the line with `numExecutors`, changing the number 2 to 3:
+   
+       `<numExecutors>3</numExecutors>`
+   
+   2. Restart the server. You will see that the number of executors has increased from the default two to three.
+   3. Plugins persist their configuration through XML files. To prove this point, look for the `thinBackup.xml` file. You will not find it unless you have installed the `thinBackup` plugin.

```
<numExecutors>3</numExecutors>
```