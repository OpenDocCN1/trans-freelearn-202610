And with that, we have a copy of the suspicious executable code for us to explore further. As a quick check to verify we carved out the correct part of memory, let's see whether the malicious string is present in the dll:

```
remnux@remnux:~/workdir$ strings module.1040.22deb88.10000000.dll | grep malicious -b
1445:very-malicious-website.com
```

That is a good indication we have the correct dll. The strings command shows an occurrence of the very-malicious-website.com URL at offset 1445 (location added by the -b flag – byte-offset – of the grep command). To see what we are dealing with here, let's next take a closer look at what this executable is up to. Let's do some malware analysis next.

# Upload executable to VirusTotal

We are going to start off our malware analysis by sending the dll to VirusTotal to see whether there are any known detections for the executable. Open a web browser, navigate to https://www.virustotal.com/gui/ and upload the dll for analysis:

![](bbox=[0.13, 0.45, 0.86, 0.85])

Figure 11.28 – VirusTotal results for extracted dll