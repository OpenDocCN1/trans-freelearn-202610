
- **System health**: Issues related to performance usually point to underlying issues that should be reviewed and rectified. If not rectified, these can affect other areas of the application or increase the effort required to extend the application. For example, a report is running slow and the architect, after investigating, finds out that the organization has many complex sharing rules. Each time the report is run, the sharing rules must be evaluated, thereby slowing down a report that otherwise would have performed well. Another area that this could impact is if your customer has integrations that run on a nightly basis to bring in data say from an ERP. Having complex sharing rules can slow down the process because the system has to recalculate data access. This can be a problem in very large global orgs where users are logged in and using the system in multiple time zones. In these situations, other techniques can be used to ensure a smooth data import and minimize the impact on users.

- **Service Level Agreements (SLAs)**: System performance is important not just from the perspective of internal users, but also of customers. Your SLAs with customers outline the response times and the level of service the customer can expect to receive. If your org is non-performant, your customer SLAs can be adversely impacted. For instance, in a large car dealership, a commitment is made to customers that their vehicles will be serviced within a certain period, and if that time period is missed, then an automatic 15% discount is applied to the entire bill. Salesforce is used to manage work orders and submit orders for auto parts. Let's say a technician is attempting to query the parts for a specific make and model so they can order them, but the system is slow, which is taking the technician longer than expected to order relevant parts. As a result, work will not get completed on time and cost the company revenue in the form of the 15% discount it now has to give to the customer.

- **Continuous improvement**: Organizations try to compete and maintain their competitive advantage and use different techniques to do so. Proactively monitoring your performance enables you to ensure that mistakes made in the past are not repeated and lessons learned in the past are incorporated for future design and build. The net result of this is an overall improvement in customer service and an assurance for internal users that the organization is taking data seriously and is vested in it.

Now that we have looked at the reasons for monitoring performance, in the next section, we will review why performance issues happen in the first place, followed by a discussion on some of the tools that can be used to measure and improve performance.

## Reasons for performance-related issues

We established earlier that Salesforce is a critical business application and is increasingly used for more business functions than just CRM. There is a multitude of factors that could impact performance and affect business operations and therefore, we must understand these factors.

The first thing to do when your users report a performance-related issue is to check the Salesforce Trust site and ensure that the problem is not on the Salesforce side. You can also subscribe to alerts from this site. To check the status of your instance, visit `status.salesforce.com` and enter your instance ID as shown in the following steps. First, let's review the steps to find your instance ID:

- Navigate to **SETUP** and then **COMPANY INFORMATION** to find your org's Instance ID, as shown in the following screenshot:

![](img/782d45700e093b3d82b348c0ccc8ccba_233_0.png)

Figure 6.1 – The Company Information page showing the instance ID, which is NA49 in this example

- Once you have the instance ID, go to status.salesforce.com and enter your instance ID:

![](img/782d45700e093b3d82b348c0ccc8ccba_234_0.png)

Figure 6.2 – All features are operating without any issues for instance NA49

You can also click on the **HISTORY** tab to see historical records for any issues that have arisen on this instance, and the **MAINTENANCE** tab shows the patches and releases applied on the instance, including upcoming releases. This can be a very helpful tool for planning purposes ahead of each release, as well as planning for in-flight or upcoming projects.

One challenge with performance issues is that there is a wide spectrum of factors that can impact performance, and therefore it is important to narrow things down quickly to the most likely culprit. A good way to start on that is to cast a wide net and ask questions such as the following:

Does it affect everyone who is using Salesforce?

- Are users in different locations experiencing similar performance issues?
- What browsers are they using?
- Is the performance issue experienced while using other browsers?

Having the answers to these questions can help you narrow down the issues to the two or three most probable possibilities, which can then be explored further. Let's look at the reasons for performance-related issues:

- **Leisure network traffic**: Many users these days carry devices that connect to the office wireless access points and stream high-definition video and audio, which can result in the office networking bandwidth getting constrained. A way to mitigate this could be to provide a separate access point for these users so leisure network traffic passes through that access point and critical applications, including Salesforce, can run on the regular network, leaving plenty of bandwidth free. You will need to determine a way to prevent users' devices from getting on the network; this can usually be done through **Media Access Control (MAC)** filtering so only authorized MAC addresses can get on the regular network.

> **MAC addresses**
> 
> A MAC address is a unique identification number that uniquely identifies hardware on the network. Every MAC is unique in a network and no two MAC addresses will ever be the same. This is ensured by the nomenclature used for MAC addresses, a topic outside the scope of this book. Read more about MAC addresses at https://www.pcmag.com/encyclopedia/term/mac-address.

- **Bandwidth quality**: Low bandwidth can be a problem and your users will notice if you try to resolve it, but adding more bandwidth to fix performance-related issues will not always result in desired outcomes. This is because the quality of the bandwidth is also important. An increase of a few percentage points in packet loss can impact performance in a noticeable way.

- **Location**: Salesforce has several hundred instances globally where all the orgs are run from. Your proximity relative to the instance that hosts your org can also be a factor in performance-related issues. If the physical distance is significant and there are many hops to get to the instance, this can result in slow performance. Salesforce instances are spread throughout the world in multiple regions and the org is assigned by Salesforce based on where the users are located. Before signing up, you should consult with your account executive if you would like your org to be in a region where most of your users will be based, rather than in the region where the contract will be executed.

### Hop

Hop is a networking term that refers to the number of network devices a packet has to pass through to reach the destination. These network devices can be routers, switches, repeaters, and so on.

Each device receives the packet, processes the information in it, and then forwards it on to its destination. Each time a device does that, this takes time and can have an impact on the performance when millions of packets are transmitted. In Windows, you can use the `tracert` command in **Command Prompt** to see the number of hops. For example, from my laptop to `salesforce.com` is 9 hops.

Keep in mind that a lower hop count doesn't necessarily mean a faster connection because other factors contribute to the connection speed as well.

- **Inefficient configuration or code**: Performance issues can also be caused by incorrectly configured applications or code that is sub-optimal. These can be a massive drain on system resources, thereby impacting performance. If a problem keeps happening in a specific Visualforce page or a Lightning component, you will want to review the code and ensure that it follows best practices. Another possibility is an incorrectly configured application. Earlier, we discussed the example of sharing rules. If there are too many sharing rules, that can slow down reports or data queries in general because every time a query is executed, the system has to run the sharing rules to ensure that the data security is protected in line with these rules.

- **AppExchange packages**: Many AppExchange packages add business value to your processes, but some of the packages also communicate back and forth with the ISV servers and can impact performance. For example, if you are using a digital signature solution and install the managed package, there may be bandwidth being utilized by the package, resulting in poor performance. Admins should frequently review and assess managed packages to ensure they are still useful and provide the value that they were initially installed for in the org.

- **Browsers**: The unsung heroes of the cloud world, browsers play a critical role in users' experiences. As a browser is used over time, it accumulates a lot of data. This can include the browser accumulating a large number of cookies and caches resulting in an impact on performance. It is also recommended to have the browsers updated to the latest release, as browser vendors are continuously releasing new patches to fix not only security vulnerabilities but also to improve performance.

Using browsers that are supported by Salesforce is another important factor. For example, an organization was using Internet Explorer 11 and kept running into issues and therefore, users started to use Google Chrome. The issue was that whenever users ran into problems with Salesforce or in general, corporate support teams would not provide support since Internet Explorer 11 was the supported corporate browser.
