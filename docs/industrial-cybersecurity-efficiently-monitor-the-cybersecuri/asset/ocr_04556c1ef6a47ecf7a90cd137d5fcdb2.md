We should change the somewhat cryptic subroutine name to something a bit more telling to help overall analysis. We can make this change by clicking on the subroutine name at the top of the view pane and hitting the N key:

![](bbox=[0.246, 0.173, 0.755, 0.52])

Figure 11.41 – IDA Pro – rename function

Rename the subroutine to something like GenerateUserAgentString. We now have an easier time following the program flow:

![](bbox=[0.31, 0.603, 0.69, 0.821])

Figure 11.42 – Malware – GenerateUserAgentString routine

Now, to see what sub_10003286 has up its sleeve, let’s take a look (double-click on the subroutine name) at its functions: