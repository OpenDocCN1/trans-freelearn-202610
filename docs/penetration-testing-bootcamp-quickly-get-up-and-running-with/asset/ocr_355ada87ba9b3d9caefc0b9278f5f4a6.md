# 3

# Setting up and maintaining the Command and Control Server

After gathering as much intelligence as possible over the last two days, it is now time to start thinking about the penetration test itself, and how we will execute it. On Day 4, we will start the actual testing or penetrate the various systems that we found during our investigational phase, but before we get there, there are still some items to think about. First, where do we plan to place any of the evidence that is acquired during the test? We don’t want to leave it on the server in case it is found and potentially changed (or the vulnerability is patched before we can report it). We may also need to perform some off-server analysis depending on the device we have within the network running the test. This is where a command and control (C&C) server comes into play.

In this chapter, we will discuss the use of a C&C server within a penetration-testing exercise and how it plays a pivotal role. We will also dive into various options that exist for a C&C server. This will allow you to have options as to where you put it as well as what it is. Finally, we will discuss setting up, managing, and monitoring as well as using this C&C server effectively and securely. This will include automating some tasks as well as file utilities to make sure we stay under the radar. We will be covering the following topics in this chapter:

+   - C&C requirements
+   - Setting up secure connectivity
+   - Inside server SSH setup
+   - C&C server SSH setup
+   - Setting up a reverse SSH tunnel
+   - Stunnel to the rescue verify the automation automating evidence collection file utilities