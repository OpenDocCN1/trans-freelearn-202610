But what if I have applications that I don’t want to be able to find specific services? Well, I’m glad you asked! In some cases, you have people or machines wishing to contact other people to talk to them about extending the warranty on their car. Perhaps I don’t want to receive those messages. Well, I would tell that central operator to not allow those callers to contact me, kind of like a prehistoric *Do Not Call Registry*. These decisions are also managed by the Consul servers in accordance with your directive.

OK, it sounds like Consul can make a lot of cool and intelligent decisions automatically, but where does it get the information required to make those decisions? Well, how is any decision made? An employee’s raise might be based upon their work performance considering they were also writing a book. What clothing we decide to wear is based on occasion and environment. What to eat for dinner can be based on…well, too many variables to list here! Every decision we make is only as good as the data we have. The Consul client provides that data.

> **Important Note**
> Within the official Consul documentation, there is a single *agent* that can be configured in both client and server mode. Throughout this text, we will refer to the server agents simply as the *server*, and the client agent simply as the *client*.

# The cluster constituents

As the brain is the decision-maker in any system, there must be entities that will feed that data and perform the work based on that decision. For example, when the manager makes their decisions, the data is likely fed by board members, senior management executives, and hopefully team members. That being said, we all know the team members are the ones that do all of the work!

The plethora of data fed into the decisions made by the Consul server structure consists of two categories:

+   - Consul configuration
+   - Network data