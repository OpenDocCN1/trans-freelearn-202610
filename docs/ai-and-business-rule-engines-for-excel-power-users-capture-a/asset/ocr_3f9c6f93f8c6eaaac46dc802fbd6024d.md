# Are you reading this book for personal or business reasons?

It is very likely that you’ll find yourself fitting into one of the following categories:

+   - **Personal use or exploration:** You own the laptop that you are trying out these samples on, and your main aim is to improve your skills. While you have complete freedom to install software (a good thing), you do not have an IT helpdesk to turn to if things go wrong (not so good).

+   - **Business user:** As one of the first people in the organization to try AI and business rules, you’re still driven by the need to improve your skills, but you may also have a business problem in mind. More likely than not, there will be restrictions on the software you can install on your laptop. On a positive note, there is an IT helpdesk to support you. While your colleagues may not understand business rules engines, they can help you work through these instructions to set one up.

+   - **Business user, joining an existing rules project:** You’ve been handed this book by the team running the business rules project to help you get up to speed. It is likely that they have much of the technical setup already completed. It is still worth reading this chapter so you know more about the technologies they have chosen.

No matter what situation you find yourself in, this book will walk you through the instructions step by step. First, we need to choose a business rules engine to install.

# Which business rule engine to use

We saw in the previous chapter that rules engines can be embedded in many places that you don’t expect (such as Excel and Outlook). There are hundreds of rule engine-like projects out there. So, how do we filter through these to pick one that can run the *hands-on* examples in this book? We’ll do this filtering based on the following criteria:

+   - We need a good user interface for editing rules, aimed at business users. This rules out *Jess* and *Prolog*, which are very powerful but need a lot of technical knowledge.

+   - The main focus of the product should be rules. For example, we’ll see other products later in the book that use workflows (for example, *Microsoft Power Automate*). These are great for doing steps in order but don’t have as powerful decision-making capabilities.

+   - Since you’re in *learning* or *trying things out* mode, you want it not only to be free to download but also free from hefty license costs in the future. We’ll explain later how open source is a good fit for this, but for the moment, it rules out fully commercial vendors such as *Trisotec*, *TIBCO*, and *FICO*.

+   - The program must be available to be downloaded and installed by an Excel power user. This excludes *Microsoft Biztalk Server*, which is a very powerful product but needs to run on the Server version of Windows.