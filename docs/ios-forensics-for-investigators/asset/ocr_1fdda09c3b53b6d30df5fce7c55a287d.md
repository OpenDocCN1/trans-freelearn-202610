# # Chapter 5: Pattern-of-Life Forensics

In the previous chapter, we learned all about different kinds of artifacts that can be found on iOS devices, such as SQLite databases and plists, and how to manually analyze these files. In this chapter, we will use this knowledge to work with some of the most interesting databases from a forensics perspective, such as the **KnowledgeC** database, which is the go-to solution for pattern-of-life forensics.

Pattern-of-life data is all about the habits that the device owner carries out in their day-to-day life. When it comes to smartphones, this includes what apps have been used at any given point in time and for how long, when the device was unlocked, what the battery temperature was, and what webpage the user was browsing.

We'll start the chapter by defining pattern-of-life forensics to get a better understanding of what kind of data we may encounter in an iOS device investigation. We will then discuss timestamps and how to convert them between different time zones. Then, we will take an in-depth look at KnowledgeC, power logs, and device events, and we will learn how to identify user interaction. Finally, we will introduce Apollo, an open source tool that greatly simplifies the process of locating and reviewing pattern-of-life artifacts.

In this chapter, we will cover the following topics:

+   - Introducing pattern-of-life forensics
+   - Working with timestamps
+   - Logs, events, and user interaction
+   - Introducing Apollo