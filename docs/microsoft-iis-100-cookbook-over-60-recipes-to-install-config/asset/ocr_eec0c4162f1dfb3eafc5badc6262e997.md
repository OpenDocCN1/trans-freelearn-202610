8. Here, we are creating a deny rule for the IP 172.16.15.65. We can add a deny rule for a specific IP, domain, or range of IP addresses. We've added a single IP for now: 172.16.15.65. Click on the OK button. We've now created the deny rule.

9. We need to configure the dynamic IP address restriction setting. Go to the IP Address and Domain Restrictions Actions pane. Click on Dynamic IP Restriction Settings. You will get a new window:

![](bbox=[0.19, 0.26, 0.81, 0.65])

10. Here, we set the deny IP address rule based on the number of concurrent requests; for example, with the first option, we can allow a single IP address to access the website with a maximum of four simultaneous sessions. More than four website access requests will be denied (terminated). With the second option, we can require that each IP be able to make only a specific number of website access requests for a specified time period. We are not configuring this part here, but when you publish your website on the internet, you can do this to protect against certain cyber attacks.