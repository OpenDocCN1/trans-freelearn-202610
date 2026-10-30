# Technical requirements

To perform the exercises and experiments proposed in this chapter, you must meet the following technical requirements:

+   - You must have integration admin access to a working Captivate Prime account, which can be a trial account.
+   - A basic understanding of programming and application development is helpful but not required.

You are now ready to discover yet another amazing set of capabilities provided by Adobe Captivate Prime LMS.

# Data Exchange Between Captivate Prime and Other Systems

All of the tools and features discussed in this chapter share a common goal: enabling the exchange of data between Adobe Captivate Prime and other IT solutions deployed within your organization. This section briefly discusses the various types of data that can be exported to and from Adobe Captivate Prime and examines some of the typical use cases in which such data exchange is useful.

# Automated user import

As discussed in Chapter 5, Managing Users, it is perfectly possible to define users directly within Captivate Prime. That being said, virtually every organization maintains a directory of users who have access to their IT infrastructure. Such a directory oversees user authentication and authorization, which is what makes it a key component of any corporate network:

+   - **Authentication** answers the question "Who are you?". It is the reason why you use a username and a password when logging into a network, but other means of authentication are also possible, such as fingerprints, face recognition, and badges.
+   - **Authorization** answers the question "What can you do?". Just because the system knows who you are, does not mean you have access to all areas of the network. Some users have administrative access and can do just about anything they want, while other users have limited access to certain network resources only.

LDAP-compliant directory solutions, such as Microsoft Active Directory, Azure ADFS, OpenLDAP, and ApacheDS, are among the most popular enterprise directory solutions used by organizations of any size.