12.  Hit **F8** a few times, until we land just after the InternetOpenA function:

![](bbox=[0.11, 0.14, 0.87, 0.33])

Figure 11.57 – OllyDbg – beaconing function – InternetOpenA function

13.  Notice how on the right side, during stepping through the program, the registers change and after the call to InternetOpenA the LastErr section shows the result of the call – `ERROR_SUCCESS`, the call was successful. The registers are the windows into program execution, they store variables, parameters, results, and other information. OllyDbg allows the manipulation of register values, which can help reverse malware.

I am going to reiterate that this exercise isn’t aimed at making you a professional reverse engineer or malware analyst but merely a means to get you familiar with concepts and methods. Find a good book to read on the subject if you are interested in learning more.