The process for using clearev is quite easy. You first need to compromise a Windows host of some sort. Once you have performed this, you will arrive at the following prompt:

```
meterpreter >
```

Once you are at this prompt, you will know you have successfully compromised a host. To see what options you have available, type help at the prompt to see all your available options. If the host is a Windows machine, you should see the clearev option under the System section. To clear out those logs, just type the command clearev at the meterpreter prompt. There are no arguments to pass. Here is the process that I followed in my lab:

```
meterpreter > clearev [*] Wiping 45 records from Application... [*] Wiping 219 records from System... [*] Wiping 5 records from Security...
meterpreter >
```

Based on the output, you can see what logs have been cleared as well as how many messages were removed.

# Shredding files with shred

When you really need to make sure that your files and partitions are gone, you may want to use shred. Unlike rm, which only removes the pointers, shred overwrites the file multiple times to try and make sure it is recoverable using any tools. This holds true for whole partitions as well.