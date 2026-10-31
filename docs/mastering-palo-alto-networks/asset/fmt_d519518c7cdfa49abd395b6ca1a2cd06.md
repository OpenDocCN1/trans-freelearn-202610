
This is different from URL Filtering via a security profile in that URL Filtering will compare the URL found in a session against all predefined URL categories and custom URL categories. This could introduce complications with overlapping categories or may make a rule too wide.

For example, if only a specific category needs to be allowed or denied, a traditional URL Filtering security profile would need to be set to allow the desired categories and block all the other ones, which may disrupt other more generic web browsing security rules. A typical example is a printer or other IoT device that needs access to a preset list of URLs to receive updates or report metrics, but no other URL categories. Using a custom URL category directly in the security rule limits that device’s access to only these FQDNs.

## Controlling logging and schedules

By default, each security rule is set to `Log at Session End`. This means that a log is only written to the traffic log once a session is broken down. For some sessions, it may be interesting to log more interactions, and so `Log at Session Start` could be enabled. This does cause quite a lot of overhead, however, as there will be a log for each new stage of a session when the SYN packet is received and for every application switch. So, there could be two to five additional log entries for a single session.

Other applications that are very chatty or less relevant may not need to be logged at all, such as DNS.

### Important note

Even with both the start and end logs disabled in the security rule action tab, any threats detected in a session will still be logged to the threat log.

Log forwarding can be used to forward logs to Panorama, Strata Logging Service, or a syslog server or to send out an email. If you name one of the log forwarding profiles default, it will automatically be added to every new security rule that is created:

![](Images/B31589_03_42.png)

Figure 3.42: Log options and schedules

`Schedule` can be used to create timeframes when this security rule will be active if certain applications are only allowed at specific times of the day (for example, Facebook can be allowed during lunch and after hours):

![](Images/B31589_03_43.png)

Figure 3.43: Schedules

Before you continue putting this new knowledge to work and start creating more rules, let’s review how you can prepare address objects so the rule base becomes more readable and you can reuse similar objects in multiple rules.

## Address objects

To make managing destinations in your security and NAT policy a little easier, you can create address objects by going to `Objects` | `Addresses`. When you create a new object here, you can reuse the same object in different rules, and if something changes, you only need to change the address object for all the security and NAT rules to be automatically updated:

1. Click on `Add` and provide a descriptive name for the address. It is good practice to set up a naming convention so that you can repeat this process for all other address objects. A good example is to prefix all server names with `S_` and all networks with `N_` so that they’re easily identifiable.

2. Set a description if needed.

3. Select the type of object that this will be:

   * `IP Netmask` lets you set an IP with a subnet mask down to `/32` or `/64` for a single IPv4 or IPv6 address (no need to add `/32`).

   * `IP Range` lets you define a range that includes all the IP addresses between the first and last IP set in the range, separated by a dash `-`.

   * `IP Wildcard Mask` lets you set a subnet masking that covers binary matches, where a zero bit requires an exact match in the IP bit, and `1` is a wildcard. So, for example, a wildcard subnet of `0.0.0.254` translates to `000000000.0000000.0000000.11111110`. The first three bytes are set, and in the last byte, all but the first bit are wildcards. This means that if the associated IP address is set to `10.0.0.2` (`00001010.0000000.000000.00000010`), all of the IPs in the subnet that end with 0 will be matched (that is, all of the even IP addresses). If the IP is set to `10.0.0.1`, all of the odd IPs would match. This type of object can only be used in security rules.

   * `FQDN` lets you set a domain name that the firewall will periodically resolve according to the `time to live` (`TTL`) and cache. Up to `10 A` or `AAAA` records are supported for each FQDN object. Use the `Resolve` link to verify that the domain can be resolved.

4. Add a tag to easily identify and filter policies for this object.

5. Click `OK`.

Once you have sets of objects that are similar, you can also create groups by going to `Objects` | `Address Groups`. These groups can be used to bundle objects for use in security or other policies.

## Tags

Tags can be leveraged to group, filter, or easily identify many other objects. Security zones, policy rules, or address objects can all be tagged with up to 64 tags per object. By going to `Objects` | `Tags`, you can create new tags:

1. Click on `Add` and create a descriptive and preferably short name for the tag (up to `127` characters). You can also use the dropdown to select one of the already-created security zones, which will cause the tags to be automatically assigned to this zone.

2. Select a color or leave it as `None`.

3. Add a comment.

4. Click `OK`.

As you can see in the following screenshot, tags can then be used to visually enhance your rule base or to filter for specific types of rules:

![](Images/B31589_03_44.png)

Figure 3.44: Tags in the security policy

While building security rules, objects (such as addresses, applications, and services) can be clicked and dragged from the object browser on the left into any rule, and from one rule to another. There is no need to open a rule and navigate to the appropriate tab to add objects.

After a while, you will want to review the security rule base you’ve built to make sure you haven’t missed any applications, left rules too open, or have any duplicates that leave rules unused. Let’s take a look at how this can be done.

## Policy Optimizer

While you’re on the `Security` or `NAT policy` tab, there’s a tool called `Policy Optimizer` in the bottom left-hand corner that can help improve your security rules by keeping track of rule usage. Policy Optimizer records statistics relating to your rules and can report the following:

* Rules that have been unused for 30 days, 90 days, or for all time so that you can delete them

* Rules that are set up with no applications defined and the applications that were accepted by those rules

* Rules that have applications that are not being used so that you can remove these excess applications

After the rule base has been in production for a while, the output of Policy Optimizer will start to resemble the following screenshot:

![](Images/B31589_03_45.png)

Figure 3.45: Policy Optimizer

In tandem with Policy Optimizer, each rule also has a column called `Apps Seen`.

### The Apps Seen column

Each security rule will also gather information about the applications passing through it. This information can be accessed from the `Apps Seen` column in the security policy:

![](Images/B31589_03_46.png)

Figure 3.46: Apps Seen

All detected applications can be added to the current rule, an existing rule, or a cloned rule from the current rule.

## Creating NAT rules
