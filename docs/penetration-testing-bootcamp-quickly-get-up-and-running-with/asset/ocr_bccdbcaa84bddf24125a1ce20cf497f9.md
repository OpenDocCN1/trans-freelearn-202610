Now that I have sslstrip running and listening, I just need to send traffic to it. I will use arpspoof for this, but could also use Ettercap as well. This will be accomplished the same way I performed the ARPspoofing attack earlier in this chapter.

I should now see SSL traffic coming my way. To get the user to click on AOL, there are a lot of different options, some of which I have already gone through in some of the previous chapters. In my case, I will send the user a phishing email to force them into clicking that link.

Once the user clicks the link and logs in to the site, I can check my log file and see that I have indeed harvested their credentials via the MiTM attack. Here is the log entry with the information in it:

![](bbox=[0.31, 0.34, 0.69, 0.47])