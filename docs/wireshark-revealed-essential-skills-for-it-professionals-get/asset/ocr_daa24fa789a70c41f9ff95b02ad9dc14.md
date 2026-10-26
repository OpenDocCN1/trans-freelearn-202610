![](bbox=[0.12, 0.06, 0.93, 0.24])

Figure 8.28: Creating a button

14. Once you click on OK, you will be able to see the button next to the Save button in the display filter toolbar area.

15. Now, whenever you want, you can create a similar display filter without typing it into the display filter box. You just need to click on the button that you created recently.

![](bbox=[0.12, 0.45, 0.93, 0.51])

Figure 8.29: The newly added button

To make this more interesting, I would advise you to create a coloring rule for the HTTP 404 error. This will definitely help you identify particular error types more conveniently.

Next, we will see another application protocol that is commonly used by various applications in order to translate a domain name to its IP address. Yes, I am referring to DNS. As we know, the DNS protocol runs over a UDP or TCP. There are various response code that relate to DNS errors that range from 0 to 21. The dissectors present in Wireshark do know about response code. Using this, Wireshark is able to show you messages relevant to the error code. To replicate an error, I will visit a website that does not exist on the Web; hence, I will receive an error. But my gateway does not know about this, so it will try to resolve the IP address associated with that name. In return, we will see a DNS response containing an error. The infrastructure is the same that we