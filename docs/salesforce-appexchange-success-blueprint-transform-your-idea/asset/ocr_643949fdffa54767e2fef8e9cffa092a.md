Now, you may be wondering how you can access this valuable Push Upgrade API. Fear not! You can wield its power directly from the command line, or you can navigate its features through the familiar terrain of Package Visualizer, which we previously explored in *Chapter 2*. Armed with this knowledge, you can confidently navigate the realm of release management, knowing you have the power to deliver seamless upgrades to your users.

# Embracing push upgrades safely and effectively

It’s common for ISVs to hesitate when considering the utilization of push upgrades. The notion of automatically transmitting updates to customers often evokes a sense of uncertainty, primarily due to concerns about potential disruptions or incompatibilities that may arise.

Despite their vast potential benefits, push upgrades often remain unexplored territory in the release process. It’s not uncommon for many customers to operate on legacy versions of applications, requiring manual updates to their orgs. This practice not only constrains the effectiveness of the release process but also increases operational overhead.

One viable solution to this predicament is the incorporation of customer-editable custom settings, such as an Opt-out for Push Upgrade setting. By default, this would allow customers to be upgraded to the latest version of the package automatically. However, it also empowers the customer’s administrator to disable the auto-upgrade feature should they prefer to maintain manual control. An example of how to implement this in a post-installation Apex script is provided here: