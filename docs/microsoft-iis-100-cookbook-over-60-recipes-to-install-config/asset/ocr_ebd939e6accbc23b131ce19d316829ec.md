Have a look at the *Installing IIS 10.0 on Nano Server* recipe to learn how to run PowerShell.

+   4. Let's connect to Nano Server on `172.16.15.63`. We have to first set up our Nano Server instance as the trusted host for **WIN2016IIS** server. We will write the PowerShell command `Set-Item WSMan:localhostClientTrustedHosts -Value 172.16.15.63 -Concatenate` and press *Enter*. You will get a pop-up to confirm the trusted host. Click on **Yes**. You have now finished the trusted host setup for Nano Server.
+   5. Let's now connect Nano Server. Run the PowerShell command `Enter-PSSession -ComputerName "172.16.15.63" -Credential ~Administrator` to connect to Nano Server. You will get the popup for the Nano Server administrator password, as shown here:

In the command `-ComputerName "172.16.15.63"`, substitute your IP address.

![](bbox=[0.307, 0.505, 0.694, 0.754])

+   6. You have to provide your Nano server password and press *Enter*. You will be logged in through PowerShell remotely.