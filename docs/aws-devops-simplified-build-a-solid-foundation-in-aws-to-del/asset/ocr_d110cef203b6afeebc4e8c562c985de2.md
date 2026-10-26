+   - Getting started with your AWS account
+   - Understanding the test application and development toolbox
+   - Building an AMI and deploying a test instance

Let’s begin with an analogy that fits our topic of discussion well.

# Technical requirements

To follow along with this chapter, you will need the following:

+   - AWS Cloud9 IDE deployed in your AWS account
+   - Active AWS account subscription
+   - Hashicorp’s Packer for AWS AMI creation

# Pets versus cattle

In the old way of doing things, servers were always cared for, lovingly nurtured, and given pet names such as Atlantis, Raptor, Battlestar, and so on. These were standalone systems that hosted critical applications, were monitored 24/7, and required administrators to nurse them as soon as issues were identified. The smallest of problems with these systems meant some part of the organization would not function, or external customers would be impacted. The nature of how these systems were perceived and maintained was similar to how we treat pets – with utmost care and affection. You cannot just replace them; instead, you must fix the underlying problem and get them healthy again. The application developers also operated with a similar mindset in terms of rolling out changes to these servers. They would keep track of all changes, execute scripts in a particular order, and define runbooks for all expected scenarios. Such an approach to maintaining critical systems introduces fear of the unknown, scope of human errors, and resistance in quickly rolling out changes to all systems. This is only good so long as the workloads are of a manageable size, say 5-10 servers. Once the scale increases, the limitations of scripting, tools, manual changes, and, most important of all, humans kick in. Think about unit-testing your bash scripts against all possible permutations and combinations of system libraries available on the target servers. How do you build a rollback mechanism for hundreds or thousands of servers? If these scripts are executed by a human, there is a high probability of common errors, such as running wrong commands in terminals, unintentionally targeting production environments, and typos resulting in loss of data – the list goes on. Not to say that Bash is imperfect, it’s just that working with it requires a very good level of understanding and experience. The foundation of these scripts has to be quite strong for the team to be able to reliably evolve them over time.

On the other hand, the new way of operating in the cloud promotes the idea of infrastructure disposability. Why modify, fix, and indefinitely maintain something that can be replaced with a brand-new server, at the click of a button? Why nurture existing servers and introduce additional operational overhead, or possibilities of mismatch in system configurations? In this model, organizations