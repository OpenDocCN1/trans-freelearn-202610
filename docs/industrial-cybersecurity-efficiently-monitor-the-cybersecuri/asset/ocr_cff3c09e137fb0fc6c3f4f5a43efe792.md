Creating a YARA rule out of the string is very straightforward. Just add it as a string definition and set a condition to find that string as is shown here:

```
```
rule backdoorExecutable
{
    strings:
        $string = "j@AYh"
    condition:
        $string
}
```
```

Save the file as `backdoor.yar`. We can now use the custom rule to find copies of `backdoor.exe`:

```
```
yara -s -r c:\ c:\workdir\backdoor.yar
```
```

We run the YARA rule on the `backdoor.exe` file to test whether it works:

```
```
pac@KVM0010110 : ~\workdir\
> yara -s -r backdoor.yar ./
backdoorExecutable ./backdoor.exe
0x19a2:$string: j@AYh
```
```

As a final check, when we run the YARA rule on a clean system, it shows that the rule is unique enough not to discover false positives.

We are now ready to start interrogating the environment to see whether we have any other systems with the infection.

# ## Remote scanning with `yara_scanner` using CrowdResponse

We could use `psexec.exe` and copy over and run YARA and scan all domain computers that way. That is a good option; however, in order to show another method and because reporting capabilities are non-existent in YARA, we will look at using CrowdStrike's **CrowdResponse** instead ([https://www.crowdstrike.com/resources/community-tools/crowdresponse/](https://www.crowdstrike.com/resources/community-tools/crowdresponse/)).