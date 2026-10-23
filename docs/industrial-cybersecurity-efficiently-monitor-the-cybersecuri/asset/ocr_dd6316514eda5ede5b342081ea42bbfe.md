The code doesn't seem to do much with the downloaded file, it just repeats downloading it indefinitely. We will verify that in the next section by debugging the running malware sample on the HMI-2 computer.

So, in summary, the malware, once installed as a service, will create a user agent string by concatenating the computer name with the string **Windows XP 6.11**. It then uses this user agent string during the retrieval process of the `serve.html` file from `very-malicious-site.com`. The retrieval process is repeated indefinitely.

# Dynamic analysis

Although we have a pretty good understanding of what the malware is up to after the static analysis, we want to verify our assumption the code does nothing with the downloaded `serve.html` file. For this, we will be attaching a debugger to the running copy of the malware on the HMI-2 computer. A debugger is software that lets us view, manipulate, and change running code with the purpose of debugging, finding bugs in the code. There are many debuggers with varying applicable functionality and areas of use. For our exercise, we will be using the famous **OllyDbg** debugger.

The following exercise details what dynamic analysis is all about. It is interacting with the malware in running condition, with the executable code running the functions. By using tools to manipulate, extract, or interact with the malware, we try to uncover secrets, behavior, and characteristics that we can use as indicators of compromise. The next section will have us use one particular dynamic analysis, debugging it while running in memory. We could also start an executable via the debugger (load it into the debugger), but this is hard to do with a service dll. Later, we will see an additional dynamic analysis tool, service emulation, fooling the malware into giving up its secrets to a service it wants to connect to but that we control.

# OllyDbg

OllyDbg is one of those tools that hackers, reversers, professionals, and enthusiasts alike are familiar with. It has been around for ages and even though it is no longer supported, the tool remains popular and is used extensively to this day. Head on over to `http://www.ollydbg.de/download.htm` to get a copy to follow along with the next exercise.