# 14

# Logging and Auditing

PostgreSQL provides a very rich logging infrastructure. Being able to examine logs is a key skill for every database administrator—logs provide hints and information about what the cluster has done, what it is doing, and what happened in the past. This chapter will explain the basics of PostgreSQL log configuration, providing you with an explanation of how to configure the logging machinery to get the information you need about cluster activity. Logs can be analyzed manually, but database administrators often also exploit automated tools that can provide a wider insight into the cluster activity. Related to logging is the topic of auditing, which is the capability of tracking who did what to which data. Auditing is often enforced by government laws, rather than the needs of the database administrators. However, a good auditing system can also help administrators to identify what happened in the database.

In this chapter, you will learn about the following topics:

+   - Introduction to logging
+   - Extracting information from logs using pgBadger
+   - Implementing auditing

# Technical requirements

You will need to know the following:

+   - How to manage PostgreSQL configurations
+   - How to start, restart, and monitor PostgreSQL and interact with PGDATA files