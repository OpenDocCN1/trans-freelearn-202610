
A good practice is to add some `catch all` rules to the end of your rule base, as in the following screenshot, once all the required policies have been added to `catch any` connections that are not allowed. From suspicious zones, one `catch all` rule should be set to `application-default` and one to `any`; this will help identify standard applications that are not hitting a security policy and (more suspicious) non-standard applications that are not using a normal port (see the `Allowing applications` section to learn about the `application-default` service):

![](Images/B31589_03_34.png)

Figure 3.34: The catch-all rules at the end of the rule base

Adding an `any`-`any` drop rule as the last catch all will override the intrazone default allow rule and may block some services, so ensure you account for these if you want to discard all unaccounted-for connections. Commonly overlooked connections are DHCP relay, IPSec, and GlobalProtect as intrazone.

You now have some rules actively dropping connections you do not want to get past the firewall, but there are more options available than just silently discarding packets. We’ll review the other options next.

## Action options

Several different actions are available to handle new connections, some of which are stealthy and some of which are noisy and informative. Choose which one to use depending on your needs:

* `Deny` will drop the session and enforce the default `Deny` action associated with an application. Some applications may silently drop while others send an RST packet.
* `Allow` allows the session to go through.
* `Drop` silently discards packets.
* `Reset Client` sends a TCP RST to the client.
* `Reset Server` sends a TCP RST to the server.
* `Reset Both` sends a TCP RST to both the client and the server.

If you check the `Send ICMP Unreachable` checkbox and the ingress interface is Layer 3, an `ICMP Unreachable` packet is sent to the client for all of the dropped TCP or UDP sessions. This can be useful to gracefully end UDP sessions as UDP does not support RST packets. It will also help the remote host to find an alternate destination or give up, whereas without the ICMP message, it may keep spamming the firewall with UDP packets.

## Allowing applications

There are generally two ways to determine which applications you want to allow:

* Creating a group of known applications
* Creating an application filter to sort applications by their behavior

From `Objects` | `Application Groups`, you can create groups of known applications that can be used in security policies, as shown:

![](Images/B31589_03_35.png)

Figure 3.35: Application Group

`Important note`

The security rule base is evaluated from top to bottom and the evaluation is stopped once a match is found, and then the matching security rule is enforced. This means blocking rules need to be placed `above` the allowing rule if there could be an overlap.

With the widespread adoption of cloud-based hosting and cheap SaaS solutions, more traditional programs are turning into web-based applications that are accessible over a web browser. This makes it harder for an administrator to easily determine which applications need to be allowed as the needs of the business change quickly. Application filters created in `Objects` | `Application Filters` let you create a dynamic application group that adds applications by their attributes, rather than adding them one by one. These attributes can be selected for “good” properties to be added to allow rules (as you can see in the following screenshot) or “bad” properties to drop rules:

![](Images/B31589_03_36.png)

Figure 3.36: Application Filter with basic attributes

Alternatively, the filter can be based on the predefined and custom tags assigned to applications, as follows:

![](Images/B31589_03_37.png)

Figure 3.37: Application Filter with tags

You can mix and match application groups and filters to build further security rules by adding them to the `Applications` tab, as you can see here:

![](Images/B31589_03_38.png)

Figure 3.38: The APPLICATIONS tab in a security rule

To create a new `Allow` rule using an application filter, do the following:

1. Create a new security rule and add a descriptive name.
2. Set the source zone to the `internal zone` that will connect to the internet.
3. Set the destination zone to `external zone`.
4. In `Applications`, add a new line and select `Application Filter`.
5. Click on all of the desired attributes and review some of the applications at the bottom. Add a descriptive name and click `OK` on the filter, and again on the security rule.

You now have an `allow` rule based on an application filter!

### Application dependencies

You may notice that when you start adding applications to a security rule, some applications have dependencies. These dependencies will appear in the `Depends On` field when you add your applications. These applications rely on an underlying protocol or build on an existing, more basic application that needs to be added and allowed in the security rule base for this sub-application to work.

They do not necessarily need to be added to the same security policy.

Starting from PAN-OS 9.1, these dependencies are displayed in the security rule. As you can see in the following screenshot, they appear when you are adding new applications and can immediately be added to the same security rule or to a different one in the security rule base.

Since the firewall will go over the security rules each time a session changes to a new App-ID, you may already have the dependency satisfied in a different rule and as soon as one of the App-IDs you set in this new rule is matched, the firewall will change the session to this new rule. In older PAN-OS versions, users will only be warned about these dependencies once the configuration is committed. You can review application dependencies for individual applications via `Objects` | `Applications` too:

![](Images/B31589_03_39.png)

Figure 3.39: Application dependencies

Now that the applications have been set, let’s look at how service ports are controlled.

### Application-default versus manual service ports

Each application will use a certain service port to establish a connection. By default, each service is set to `application-default`, which forces each application to use its default ports (for example, web browsing uses ports `80` (unsecured) and `443` (SSL) secured, while FTP uses ports `21` (unsecured plaintext) and `990` (secured)).

`Important note`

Protocols that use pinholing, such as FTP, are automatically taken care of via the `Application Layer Gateway` (`ALG`), which is a part of the content decoder that is specific to this protocol. The ALG ensures “child” sessions are accepted as part of the same security rule even though the port may be different from the default port.

If an application needs a custom port, you can add a manually created service object, but this would prevent the use of `application-default`. So, any exceptions should preferably be made in individual rules to prevent applications from “escaping” via an unusual port:

![](Images/B31589_03_40.png)

Figure 3.40: Service ports

Adding a URL category can be used to allow or block URL categories at the TCP layer:

![](Images/B31589_03_41.png)

Figure 3.41: URL Category in the security rule

Rather than applying URL Filtering in (ISO) Layer 7, a URL category added directly to the security rule simplifies allowing or denying access to a specific category.

When an outbound connection is identified as web browsing (or SSL), the content decoder will identify the URL that is being accessed and a category lookup will take place. If a URL category is set in the security rule, as illustrated in `Figure 3.38`, a Layer 3 action can immediately be applied to allow or deny a session from proceeding.
