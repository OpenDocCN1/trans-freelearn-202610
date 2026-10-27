![](bbox=[0.253, 0.099, 0.72, 0.654])

Now that we know *what* we can do with individual rules, let's look at *where* we can (or can't) perform these actions… which brings us to rule types. The following is a list of all the rule types currently supported. Although we will only be using a few of them during this exercise, I wanted to list them all simply for completeness:

+   - **ACL resource:** Controls the management of ACL rules.
+   - **Agent:** This rule type controls aspects related to managing the Consul agent, such as joining a cluster, listing members, and agent configuration. Agents really should have write access to their own nodes; otherwise, they won't be able to register their own service information with the catalog.