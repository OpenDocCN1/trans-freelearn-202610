Then, on the resulting page, run the test. As shown in Figure 6.4, this produces the Octane score, the download and upload speeds, and other pertinent information. In this case, the Octane score is well below the minimum score required by Salesforce, so the user should expect to experience performance issues:

![](bbox=[0.21, 0.19, 0.79, 0.64])

Figure 6.4 – The Octane score

As seen in Figure 6.4, Speedtest has other information that is useful for troubleshooting, such as upload/download speeds, instance ID, and other pertinent details relating to the browser and the operating system.

> **Octane score**
> 
> The Octane score, developed by Google, is a measure of how JS in the browser is performing. Higher Octane scores correspond to faster load times. It is important to note that the Octane score factors in hardware and the type of browser as well. Salesforce requires a minimum Octane score of 20,000 and recommends an Octane score of more than 30,000 for optimal performance.