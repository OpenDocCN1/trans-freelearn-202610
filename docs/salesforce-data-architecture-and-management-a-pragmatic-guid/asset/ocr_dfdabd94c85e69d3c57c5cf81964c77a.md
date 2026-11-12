Let’s discuss some of the tools at our disposal to monitor performance in Salesforce.

# URL suffixes

This is a technique to view your page load times for LEX. It is done by adding ?optVisible=1 to the end of the URL. For example, when I add this to the **Account** page for a specific record, the result shows me that the page was loaded in **4.49** seconds. We will discuss the details related to LEX performance in the *Performance in LEX* section.

We can see in the top right of **Figure 6.3** that the **Account** page took **4.49** seconds to load:

![](bbox=[0.116, 0.295, 0.853, 0.465])

Figure 6.3 – The EPT (Experienced Page Time) value is shown as 4.49 seconds

This is a handy tool to quickly get some preliminary data on how the page is performing without going through any extensive setup.

# Speedtest

This is a test to measure performance and provides different metrics to gauge **JavaScript (JS)** speed and the connection speed to Salesforce servers. To use this, simply append /speedtest.jsp to the URL while logged in to your org. For example, in my case, I have https://curious-wolf-q1n89f-dev-ed.my.salesforce.com/speedtest.jsp.