The argument to be provided to this command is the path to the directory that  
contains the `inspec.yml` file. Here, in this command, we use `.` (dot) in the  
argument to indicate that the `inspec.yml` file is in the current directory, and the  
following screenshot shows the result of the execution of this command:

![](bbox=[0.1396, 0.1836, 0.8672, 0.3047])

Figure 14.4 – InSpec check profile

+   4. Finally, we execute InSpec to execute the tests with the `inspec exec` command,  
+   as follows:

```
inspec exec . -t azure://
```

This command takes as an argument the path of the directory that contains the  
`inspec.yml` file (here, it is the dot). We also add the `-t` option, which takes the  
value of the tests to the target—that is, azure.

The following screenshot shows the result of the execution of this command:

![](bbox=[0.1396, 0.5195, 0.8672, 0.6906])

Figure 14.5 – InSpec exec tests

We can see from this result that all of the tests are green and therefore successful, so  
the compliance of the infrastructure is very successful.

We have just explained in this section how to write InSpec tests and run them to verify the  
compliance of our Azure infrastructure.

InSpec is a very powerful tool; it also allows you to test the configuration of a VM. That’s  
why I invite you to view the following documentation: https://www.inspec.io/  
docs/.