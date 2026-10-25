# More tools of the trade

w3af is a powerful tool, but works better in conjunction with the following tools:

+   - **Nmap** (http://nmap.org/): A simple to use, highly popular, award-winning network scanner.
+   - **Nikto** (http://cirt.net/nikto2): A Perl script that quickly summarizes system details and looks for the most obvious defects.
+   - **Skipfish** (https://code.google.com/p/skipfish/downloads/list): A C program that bashes away at many requests over a prolonged period. You can choose from different dictionaries of attacks. This is an excellent poor man's stress test; if your system stays up, you know that it has reached a minimal level of stability.
+   - **Wapiti** (http://wapiti.sourceforge.net/): A Python-based script that discovers attackable URLs and then cycles through a list of evil parameters.

Jenkins is flexible, so you can call a wide range of tools through scripts running in jobs, including the security tools mentioned:

There are a number of great resources for securing native OSes, including the Debian security how-to (https://www.debian.org/doc/manuals/securing-debian-howto/); for Windows, articles found underneath the MSDN security center (http://msdn.microsoft.com/en-us/security/); and for Mac, Apple's official security guides (https://www.apple.com/support/security/guides/). Online services need vigorous attention to their security surface.