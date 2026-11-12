
## The Business Process Flow Diagram

The **business process flow** diagram will help you illustrate and communicate the targeted user experience for a given process. This is a *good-to-have* diagram during the review board as it adds value. However, it also takes considerable time to create.

Practice creating this type of diagram and decide if you will include them as part of your generated artifacts or not. Remember that these diagrams are created to help us explain the end-to-end solution, not just for the sake of creating diagrams.

In real life, diagrams that explain each business process are a must-have. It is also strongly recommended that it is in standard BPMN 2.0 format as this creates a common language between team members and avoids missing details and precision.

This diagram should include the following:

- The systems should be included in a particular process flow. This is usually represented as swim lanes.
- The starting action and actor of the process.
- Activities included at each stage, and whether they are manual or automated.
- The logic that controls moving from one step to the other or from one system to the other should be included.
- Data that has been exchanged between systems should be included.

Now, look at an example. The business process diagram in *Figure 1.8* describes a partner onboarding process. You can add as much detail as you wish for the given hypothetical scenario. In real life, this diagram is likely to include much more detail:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_51_0.png)

Figure 1.8 – Business process flow diagram example

Subprocesses help create reusable elements. It is recommended that you use subprocesses whenever applicable. However, this is perhaps more suitable for real-life scenarios rather than the review board.

## Development Life Cycle Diagram

Governance is another key knowledge area that you must cover as part of your end-to-end solution. Part of it is your environment strategy; that is, what kind of environments you are planning to use and for what. If you do not create this diagram as part of your presentation, then you are likely going to be asked to draw it during the Q&A session. This diagram is a must-have in real life and can also drive some budget-related discussions. This diagram should include the following:

- The environment types that are being planned at each stage.
- Justification for sandbox-type selection and how this is associated with the test plan. However, this probably is not going to be something you document in the diagram itself but rather mention during your presentation.
- The activities you planned for each environment and the types of tests included.
- Details about who will be deployed to each branch and at what stage.
- Make sure you fully understand the concepts of **continuous integration and continuous deployment (CI/CD)**. Hands-on experience is very important.
- Any third parties that you are planning to use (for example, automated build tools, source controls, and so on).

You will cover examples and more details related to this diagram in the chapters to come in this title.

It is common to combine this diagram with the optional source code branching diagram, if you decided to create one.

## The Contextual SSO Flow Diagram

Most of the CTA exam scenarios will have SSO requirements. This diagram (you might need more than one, depending on the used SSO standards) will help you walk your audience through the details of the target user experience. If you did not create this diagram as part of your presentation, then you are likely going to be asked to draw it during the Q&A session.

A standard sequence diagram is highly recommended. This is one of the diagrams I personally recommend as it is more suitable to be drawn interactively (for instance, using a whiteboard).

For this diagram, take the following into consideration:

- You need to know how to fully draw the following flows at the back of your hand, along with all the required details:
    - SAML IDP initiated
    - SAML SP initiated with deep linking
    - OAuth 2.0/OpenID Connect web server/Auth code
    - OAuth 2.0/OpenID Connect user-agent
    - OAuth 2.0/OpenID Connect refresh token
    - OAuth 2.0/OpenID Connect JWT flow
    - OAuth 2.0 Asset token flow

You need to understand when to use each of these flows.

- Pay extra attention to the use cases when you need to include them as part of your integration architecture (for example, an external system authenticating into Salesforce) or mobile architecture (for example, a native app authenticating into Salesforce).
- Social sign-on comes with a few caveats that you need to keep an eye on and be able to explain.

You will come across detailed examples of this diagram in the other chapters of this book. It is recommended that you include information regarding the data that is exchanged at each step of the sequence diagram.

## Summary

In this chapter, you got a better understanding of the typical CTA profile. You learned how this book is structured and how you can make the most of it, as well as how to utilize your day-to-day activities to train for the CTA review board. You now have a better understanding of the nature of the exam and some of the key considerations to keep in mind. You also took a deep dive into some of the key tools and artifacts that can help to create a secure and scalable end-to-end solution and communicate it with your audience.

In the next chapter—Chapter 2, Core Architectural Concepts: Data Life Cycle—you will dive deeper into some key architectural skills and knowledge areas. These skills are common for enterprise and software architects, and you are going to understand how they are applicable to the Salesforce Platform.

## Core Architectural Concepts: Data Life Cycle

This Book Comes with Free Online Content

With this book, you get unlimited access to web-based CTA exam prep tools like flashcards and exam tips.

![](img/a3b1eecf3c87c814267f2877e8a8be1a_55_0.png)

Figure 2.1 – CTA online resources dashboard

To unlock the content, you'll need to create an account using your unique sign-up code provided with this book. Refer to the Instructions for Unlocking the Online Content section in the Preface on how to do that.

## Accessing the Online Content

If you've already created your account using those instructions, visit packt.link/ctabookwebsite or scan the following QR code to quickly open the website.

![](img/a3b1eecf3c87c814267f2877e8a8be1a_56_0.png)

Figure 2.2 – QR Code to access CTA online resources

Once there, click the Login link in the top-right corner of the page to access the content using your credentials.

As you learned in the previous chapter, the expectations are high for a Salesforce CTA. The architect is expected to have a deep understanding of the platform's features and capabilities. In addition to a deep understanding of particular architectural concepts, the architect is also expected to have a breadth of knowledge across many architectural domains. Data is one of the key architectural domains that architects need to master. Data is all around you in your daily life, and every day, you discover new potential for it. From sales to services, marketing, business intelligence, and artificial intelligence, data is at the heart of today's modern systems.

In this chapter, you are going to cover the following main topics:

- Differences between classic RDBMS and Salesforce
- Understanding data governance
- Understanding data security
- Data regulatory compliance
- Exploring data categories
- The nature of data warehouses and data lakes
- Choosing the right document management system
- Understanding data architecture concepts
- Designing and documenting your data model
- Using database relationships

## Differences Between Classic RDBMS and Salesforce

People have used databases in their day-to-day activities for centuries. Although they have only been given the name “databases” recently, they have been developed for years with more and more use cases invented for them. Most modern applications utilize a database of some sort. Theoretically, a database is simply a collection of related data. The software system that manages this data is called a **database management system (DBMS)**. The DBMS is also responsible for controlling access to the database.

Databases have evolved over the years from simple file-based systems to sophisticated cloud-based relational database management systems and in-memory databases.

Understanding the problems of file-based systems could help you avoid challenges that could occur in modern database systems. **File-based systems** were designed for a specific set of use cases. This was primarily driven by an attempt to digitalize the activities that are usually done. Take the manual filing system as an example. Enterprises and some libraries used to have an organized way to store the different files and books they owned. A typical arrangement would see these assets stored in labeled cabinets. The cabinets themselves might have locks to control who could open them and who could not, and an entire set of cabinets could be kept in secured rooms or areas to ensure the right security measures were taken. The simplest way to find a document in this arrangement would be to go through all the documents one by one until you found what you were after.

Eventually, indexing systems were used to help locate a specific file more quickly. In other words, you could have divisions in the filing system or a summarized index sheet that pointed to the location of each stored document.

This system works as long as only a small number of items are stored, or if all you want to do is simply store and retrieve items. You can probably imagine the complexity of retrieving cross-reference data or trying to get any intelligence out of the gathered data.

**Relational databases** became very popular at the end of the last century. They dominated the enterprise landscape and are still relevant today. The data in RDBs is split across multiple _related tables_, where each row of these tables has a _unique identifier_ called a **primary key**. Related tables can be linked by referencing those primary keys with **foreign keys**. Eventually, the concept of an **alternate key** or **secondary key** was introduced. Like the primary key, a secondary key contains a unique identifier for each row. However, for one reason or another, it has not been chosen as the primary key for the table.

In the Salesforce world, each object has a unique ID field that represents a primary key. Lookup and master-detail relationship fields act as foreign keys for the child object. Unique external keys are a good example of alternate keys.
