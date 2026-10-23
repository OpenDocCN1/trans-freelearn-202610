If a quick fix can be applied, you can do so by right-clicking the incident and choosing Apply All Quickfixes.

![](bbox=[0.236, 0.13, 0.73, 0.3])

Quick fixes can be applied to a single incident, all incidents in a file, or on increasingly higher levels.

# Upgrading your application server

Now, you have your source code upgraded to the Jakarta namespace and updated your dependencies. You have also upgraded to a newer version of your application server.

Should you use a different application server than Payara in your projects, you can use the information in this section to choose the correct version of the application server to use.

There are two versions of Jakarta EE9: Jakarta EE 9 and Jakarta EE 9.1. There is no functional difference between the two; they do not contain distinctive features. However, as the migration was such a huge effort, it was decided that compatibility against Java 11 was only introduced in Jakarta EE 9.1. Thus, if you plan to run your application server on Java 8, a Jakarta EE 9 server will suffice; otherwise, use Jakarta EE 9.1 on Java 11 and higher.

The following table lists some of the more common application servers.

| Application server | Jakarta EE 9-compatible | Jakarta EE 9.1-compatible |
|---|---|---|
| Eclipse Glassfish | 6.0 | 6.1 |
| Open Liberty | 21.0.0.12 | 21.0.0.12 |
| WildFly | 23.0.2 | 25.0.0 |
| Payara Enterprise Server | 6.2021.1 | 6.2021.1 |
| IBM WebSphere Liberty | 21.0.0.12 | 21.0.0.12 |

Table 3.2 – Jakarta EE 9 compatible application servers