
The purpose of recording changes, including the order in which you made the changes and things like reboot/restart steps, is two-fold. First, you want to be able to reproduce the change in exactly the same way in multiple environments. Second, if things don't go the way you expect them to go, it's a lot easier to solve the resulting puzzles if you know exactly what you did.

(By the way, we are terrible at following this advice, which is why we are in development instead of operations.)

## Snapshots

If you can arrange to test your changes in a virtual machine environment, then your virtual machine manager will surely have the capability to make at least one snapshot. A snapshot for a virtual machine is a file-based capture of the entire virtual machine state at a given point in time. Some platforms will allow a single snapshot while others will allow arbitrarily many in a branched, hierarchical topology.

The idea of a snapshot is that you can discard all changes back to the point of the snapshot. All mistakes are conveniently erased. That's good and bad. You get a chance to start over, but you should first capture as much information as you can about what went wrong so that you don't waste too much time re-making the mistakes.

## Backups

We're not going to say too much about backups because the benefit is pretty obvious. If your configuration management system doesn't include doing a backup of configuration data before a change is made, you should definitely do it manually in any environment that is important enough that you don't want to lose it.

Backup of business data is often a decision made based on a risk assessment if there is significant downtime involved. The sheer volume of data and complex relationships can make planning for business data backup a complicated matter. For many good suggestions and best practices, there is an IBM Redbooks publication called *Disaster Recovery and Backup Solutions for IBM FileNet P8 Version 4.5.1 Systems*, available at http://www.redbooks.ibm.com/abstracts/sg247744.html.

## Looking for trouble

There are a lot of ways to categorize problems, but the most useful in our experience is to separate out initial "get it running" problems from problems that suddenly occur in what was a healthy environment. When you ask an experienced troubleshooter why something doesn't work in your environment, among the first questions they will ask is whether it was working before. If it was working before, the very next question will ask what changed. There is a rich history of people saying that, ahem, nothing changed. There's probably no reason for you to have a different answer unless you want to get your problem solved.

## Initial configuration

The odds are pretty good that your business situation is at least a little bit unique. The odds are equally good that the fundamentals of your technical environment are not so unique. That's good news because it means that the documented configuration steps are likely to fit your environment.

We're not going to admonish you to read the documentation. We honestly assume you actually do that. However it presents itself to end users, ECM software is complex. It does fundamentally complex things and often interacts with other complex software components in the environment. Seemingly small errors in configuration, which you might reasonably expect to result in a concise and enlightening diagnostic, can instead result in silence or a frustratingly uninformative error message.

CM applications depend upon the CE (and, in some cases, the PE) functioning properly. The PE depends upon the CE and the database functioning properly. The CE depends upon the J2EE application server, the database, and the enterprise directory functioning properly. The CE also depends upon the operating system and physical hardware, but problems in those areas are less common.

You will want to double and triple check your CM installation and configuration steps. Depending upon the particular environmental components you are using, those steps can be quite complicated. Although you may have used the steps in the early chapters of this book as a quick way to get things going, you will want to consult the IBM product documentation if something goes wrong. Pay particular attention to the specific releases and fix pack (or equivalent) versions in use. Check each one in the hardware and software guide to make sure they are supported for CM. For example, a common source of mysterious problems is the use of an unsupported JDBC driver.

The CE insists on authentication for all requests, and authentication failures fall into two categories: authentication failures (E_NOT_AUTHENTICATED, FNRCE0040E) and anonymous requests (SECURITY_ANONYMOUS_DISALLOWED, FNRCS0001E). Those codes we just mentioned appear as part of CE-generated exception messages. If a failing application doesn't display them, then you will have to consult CE trace logging.

Authentication failures are fairly straightforward and most often occur in isolated cases for individual accounts. These can be cases of expired passwords or other directory conditions. The CE intentionally avoids giving details of the reason for an authentication failure in exception messages because information that is helpful to a user or an administrator is also helpful to a security attacker. You may find additional information about authentication failures logged in the CE error log.

Anonymous requests are very simple to understand, but it can be tricky to track down the thing that needs fixing. The CE considers a request to be anonymous if there is no JAAS Subject, if the JAAS Subject has no user Principal, or (in the case of some application servers) the JAAS Subject contains a special Principal that the application server uses to indicate anonymity. If you haven't guessed already, these cases occur when using EJB transport. Authentication is usually done on the client side for EJB transport, and it will usually result in a simple authentication failure if the problem is something like bad credentials. Here are some things to check in the case of failure due to anonymous requests:

- If a custom application is failing, it might not be performing authentication at all. Authentication is done via an explicit JAAS login, a call to UserContext.createSubject, or container-managed authentication. Problems with the first two will be found during development and routine testing. For container-managed authentication, check that whatever mechanism you are using is actually wired up correctly. For example, we showed steps for configuring CE's container-managed authentication in Chapter 5.
- You may have an inappropriate JAAS configuration file. The application servers all have their own login modules for use with EJB transport. It would be unusual to have the modules for more than one application server actually available in a given environment, but it's not impossible. More commonly, CEWS transport uses a CM-supplied JAAS login module that won't work with EJB transport. In all of these cases, the legitimate JAAS Subject created in the client will not be recognized by the application server's EJB container, and the Subject will be discarded.
- If your client runs inside an application server, you might have neglected to establish a trust relationship between the client application server and the application server where the CE runs. In the absence of a trust relationship, the CE application server's EJB container will not accept a JAAS Subject created on the client application server. Different application servers have different procedures for creating trust relationships. Whatever the mechanism, if things were working and suddenly stopped working, check for expiration of security certificates or other artifacts of the trust relationship. They sometimes have a defined validity period that can sneak up on you.
- See if your application server has specific trace logging for authentication matters. The authentication handshake can often be more complex than you imagine, and there could be sand in the gears of any one of several moving parts.

## WAS bootstrap information

The flexibility of possible WebSphere configuration topologies can lead to a misunderstood failure mode in authentication or even basic connectivity for EJB transport. Have a look at the port assignments information for WAS in Chapter 2. It quite often happens that the names in the Host column will be localhost or another simplified form of the server name where WAS is installed. You should use the fully-qualified host name in that column. The important part is that the name that appears in those entries can be resolved properly in DNS from the client machines. It is not enough that they are correct names from the WAS server's point of view. In some exchanges, the WAS client will ask the WAS server for one of those host names and port numbers. If it cannot be resolved from the client side, the subsequent connection will fail.

## Performance

There are a lot of different ways that performance problems can arise. For a newly deployed custom application, there could certainly be less than optimal development practices that lead to performance problems. In this section, however, we will focus on troubleshooting performance problems that arise in a previously healthy system.

In addition to the items mentioned here, you will want to consult the FileNet P8 Performance Tuning Guide mentioned previously.
