# Using discovered indicators of compromise to search the environment for additional suspect systems

Now that we have narrowed down our list of suspicious artifacts to a single verified malicious executable, backdoor.exe, as was found on Workstation12, let’s see whether we have this executable running or present on other systems.

If we were to just search for the backdoor.exe filename on all the hard drives in the environment, we would miss copies of the malware that have a different name. What we want to do is look for something that makes the executable stand out from the crowd, such as a specific string or sequence of bytes that is unique to that executable, and ideally is difficult to change. This is how, fundamentally, antivirus scanners work. They have a large database of unique strings/byte sequences, called **patterns**. If a specific pattern is found within a file, this would be an indicator that the file is the malware that the pattern was extracted from.

Short of writing our own virus scanner pattern rules, we can use a tool that was designed to run a similar search on hard drives, called YARA. We will discuss YARA next.

# Using YARA to find malicious executables

**YARA** is a tool that can be used to identify files that meet certain conditions and contain certain patterns. YARA is a tool aimed at (but not limited to) malware researchers trying to identify and classify malware samples. YARA allows you to detect malware (or whatever file you are looking for) with the help of textual or binary patterns (descriptions). Each description, or rule, consists of a set of strings and a Boolean expression that determine its logic. You can define the definition in so-called YARA rule files. More information on **YARA**, as well as a download link, can be found here: https://virustotal.github.io/yara/.

Let’s see the tool in action. We will be creating a YARA rule file, test.rule, and entering a test rule to illustrate how YARA finds patterns.

Create a file called test.rule and enter the following content:

```
```text
rule ExampleRule
{
	strings:
		$my_ascii_string = "secret_to_find"
		// The 'wide' keyword indicates the string is unicode
```
```