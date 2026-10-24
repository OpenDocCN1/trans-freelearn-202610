The first type to emerge were **centralized systems**, such as **Subversion (SVN)**, **Concurrent Version System (CVS)**, and **Team Foundation Version Control (TFVC)** and Microsoft **Visual SourceSafe (VSS)**. These systems consist of a remote server that centralizes the code of all developers.

We can represent a centralized source control system like this:

![](bbox=[0.3291, 0.218, 0.6453, 0.4008])

Figure 6.1 – Centralized source control

All developers can archive and retrieve their code on the remote server. The system allows better collaboration between teams and a guarantee of code backup. However, it has its drawbacks, such as the following:

+   - In case of no connection (for a network problem or internet disconnection) between the developers and the remote server, no more archiving or code recovery actions can be performed.

+   - If the remote server no longer works, the code, as well as the history, will be lost.

The second type of VCS, which appeared later, is a **distributed system**, such as Mercurial or Git. These systems consist of a remote repository and a local copy of this repository on each developer’s local machine, as shown in the following screenshot:

![](bbox=[0.3297, 0.7055, 0.6469, 0.8758])

Figure 6.2 – Distributed source control