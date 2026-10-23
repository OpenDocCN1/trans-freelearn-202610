# 18

# Logical Replication

In the previous chapter, we talked about WAL segments and physical replication in synchronous, asynchronous, and cascading modes. In this chapter, we will cover the topic of logical replication. We will look at how to perform a logical replica, how a logical replication is different from a physical replication, and when it’s better to use logical replication instead of physical. We’ll also see that logical replication can be used to make a PostgreSQL hot upgrade. This chapter is intended as an introduction to logical replication; for further information, refer to more advanced texts, such as *Mastering PostgreSQL*, by Hans-Jürgen Schönig.

This chapter covers the following topics:

+   - Understanding the basic concepts of logical replication
+   - Comparing logical replication and physical replication
+   - Exploring a logical replication setup and new logical replication features on PostgreSQL 16

# Technical requirements

For this chapter, you will find three Docker environments in the repository:

+   - `chapter18_logical_clear`: contains two PostgreSQL installations ready for the configuration of a new logical replication.
+   - `chapter18_logical_ready`: contains two PostgreSQL installations with a new logical replication already active.
+   - `chapter18_physical_logical`: contains three PostgreSQL installations with a new logical replication made using a physical replication.