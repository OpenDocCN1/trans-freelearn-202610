C&C server setup is very much dependent on the penetration tester. Some, like me, prefer to have a centralized C&C server, with smaller devices within the network to perform tests. Others may choose to just use their more powerful devices for both testing and analysis. It's really up to the penetration tester's personal style. The following section reflects my personal style; however, you should definitely explore other ways based on your own personal preferences.

# Command and control servers

Command and control servers, or C&C servers, have a negative perception due to their extensive use for malicious purposes, such as delivering malware or other malicious payloads. For the same reasons they are used in a negative manner, they can also be used in a positive way. These C&C servers serve as the perfect way to get the data or evidence you are collecting to an offsite server so that you can review, arrange, and finalize it. You never want to leave any evidence that you were on a particular system or even leave any files or findings behind. This is where C&C servers come into play, as they can be a single source for all your penetration-testing evidence.

Where your C&C server resides is really up to you. Most of the time, C&C servers reside in someone's cloud. This could be within AWS, Azure, or any other cloud flavor of your choice. However, it doesn't have to be a cloud-based solution. You could have a server in your company office or even within your home lab in which you have that secure channel back. The choice is yours. The important thing is that it needs the following:

+   - Lots of connectivity
+   - The ability to access it remotely
+   - Access to the local on-premise box
+   - It must be cost-effective

C&C servers can be anywhere based on your preference. But even though they can be anywhere and anything, there still need to be some requirements to make sure you have an effective setup that will help streamline your penetration test, not hamper it.