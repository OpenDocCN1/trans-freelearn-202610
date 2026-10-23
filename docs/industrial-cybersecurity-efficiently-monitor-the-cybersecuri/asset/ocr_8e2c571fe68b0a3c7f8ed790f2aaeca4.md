I would say that is a pretty good sign the dll is malicious. We now have several names and classifications behind our intruder and can use this information to find specifics and uncover any additional IOCs to help us find other potentially infected machines. Instead, I would like to take you to the rest of the malware analysis process as to show how one would uncover any (additional) indicators of compromise on their own.

# ## Rudimentary inspection of the suspicious executable – malware analysis 101

Remember that the steps and procedures that follow are merely to illustrate the overall process of malware analysis and are by no means meant to be used as a detailed malware analysis tutorial. This book is not intended to go that deep but folks who are interested in learning the fascinating discipline of the malware reversing process should purchase a copy of the book *Practical Malware Analysis: The Hands-On Guide to Dissecting Malicious Software*, by Michael Sikorski.

Fundamentally, there are two approaches or phases of malware analysis: static analysis and dynamic analysis. The first has us looking at the malware without running it, just to see whether we can find hints or clues as to what the malware does. The second, more dangerous method has us run the malware and look at the environment the malware is running in to see what changes the malware makes or what artifacts it leaves behind. Let’s start our journey with static analysis. We will be using the REMnux VM for our venture into malware analysis.

# ### Static analysis

With static analysis, we aim at uncovering details, information, and specifics about the malware that gives us an insight into what its intended purpose is. There are many ways to get this data from the malware; we will be looking at three often-used techniques: dumping the malware’s strings, looking at its imports and exports tables, and disassembly of the malware executable (byte or binary) code to show its program flow.