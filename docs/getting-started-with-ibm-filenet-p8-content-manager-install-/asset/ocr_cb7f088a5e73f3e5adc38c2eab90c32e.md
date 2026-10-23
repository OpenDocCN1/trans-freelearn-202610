You can either live with that (and have a little chuckle every time you forget), or you can use the provided options of Console administration | Change console administrator login and Change console administrator password to something you will remember and that is possibly a little more secure. Although the default credentials don’t get you very far, they get far enough that some prankster could change them and lock you out.

> As we will manage only a single directory from this console, we changed the credentials to the same as the directory server administrator, cn=root.

At this point, we have a functioning TDS console server, but it doesn’t know about any actual directories to administer. The next step is to tell it there is a directory running on the same server. Navigate to Console administration | Manage console servers. Click the Add button.

![](bbox=[0.16, 0.4, 0.8, 0.66])

We used the Hostname localhost because the directory server is running on the same server as the console server. For the same reason, we left the Enable SSL encryption option unchecked. That’s for communication between the console and the directory server. If you were remotely managing directory servers, you would probably want to enable TLS/SSL.