3. Restart stunnel on the Raspberry Pi. This will make sure the updates that were just made will take effect:

```
service stunnel4 restart
```

4. Now, we just need to make sure that stunnel will start on boot up as well:

![](bbox=[0.22, 0.23, 0.78, 0.55])

Now, everything should be set up and running. However, we need to test to make sure that all this gets set back up automatically following a reboot of the Raspberry Pi, since that will be tucked away within the environment you are performing the penetration test on, and you most likely will not have access to it.

# Verifying automation

Verifying that everything works as planned is crucial to the success of the penetration test. The way that I test is to reboot the Raspberry Pi, or whatever your internal server is, to simulate a power outage within the organization.

To make sure that everything is working as planned, I start with a tcpdump running on the C&C server. This tcpdump will look for SSL traffic coming over the stunnel tunnel:

```
tcpdump host 192.168.33.123
```