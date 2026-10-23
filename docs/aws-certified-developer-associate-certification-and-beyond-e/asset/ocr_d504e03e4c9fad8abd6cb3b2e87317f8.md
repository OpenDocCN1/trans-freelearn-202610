+   2. In the top search box, type in **AWS Organizations** and select the service from the drop-down menu, as per the following screenshot:

![](bbox=[0.242, 0.154, 0.779, 0.319])

Figure 1.13 – Searching for services using the search box on the AWS Management Console

+   3. You will be directed to the **AWS Organizations** console page.

+   4. Click the **Create an organization** button in the top-right corner of the screen. This will create your AWS organization and configure your AWS account to become the primary root management account, as shown in *Figure 1.14*. This account can then host other member AWS accounts, apply security policies, and act as a central billing account for all members’ AWS accounts.

![](bbox=[0.147, 0.469, 0.871, 0.721])

Figure 1.14 – AWS Organizations

Now that you have created your AWS Organizations, it’s time to configure some OUs. These OUs enable us to manage multiple accounts that share common workload types, for example, software development environments such as development, testing, and production.