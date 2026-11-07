In the next example, `wafw00t` was unable to figure out the exact WAF being used but still lets you know one exists:

![](bbox=[0.26, 0.15, 0.73, 0.42])

# Protocol fuzzing with DotDotPwn

DotDotPwn is a slick multi-protocol fuzzer to discover traversal directory vulnerabilities within web servers. Fuzzing is the testing technique of looking for poor coding or security loopholes in software applications such as web servers or even operating systems. Because of this, DotDotPwn makes a good reconnaissance tool for finding various issues within the web server stack that you can later exploit.

Getting the most information about the environment now makes the exploitation phase much easier. We will note everything we can find and then exploit it at the appropriate time. Do not rush through the reconnaissance phase, as it will just lessen the overall quality of the penetration test. The more we can find now, the more we can exploit later.

First thing to know about `dotdotpwn` is that it supports many different protocols or modules. We will focus on the `http` module during the lab but there are other modules available such as `tftp` and `ftp`. Here are the available switches when running `dotdotpwn` from the CLI: