

## Using Database Relationships

One of the goals of the normalization process is to remove data redundancy. To do that, you must divide tables into multiple related subject-based tables. There are three main types of relationships—one-to-one, one-to-many, and many-to-many. Take a look at these in more detail:

- **One-to-one relationship**: In this type of relationship, each record in the first table has one—and only one—linked record from the second table. And similarly, each record from the second table can have one—and only one—matching record from the first table. In this relationship, one of the tables is effectively considered an extension of the other. In many cases, the additional attributes will simply end up being added to one of the tables. That is why this relationship is not very common. However, it still has its valid cases, including splitting the data for security reasons.

In Salesforce, this is particularly useful considering the rich sharing model available. You can implement one-to-one relationships in many ways, with the most common being to create a lookup relationship from each object to the other, with an automation script (trigger, workflow, process builder, and others) to ensure both lookup fields are always populated with the right values and each object is linked to its counterpart:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_84_0.png)

Figure 2.12 – One-to-one relationship

- **One-to-many relationship**: This common type of relationship occurs when a record in one table (sometimes referred to as a **parent**) is associated with multiple records of another table (sometimes referred to as a **child**). There are plenty of examples of this, including a customer placing multiple orders, an account with multiple addresses, and an order with multiple order line items.

To have this relationship, you would need to use what are known as primary keys and foreign keys. In the child table, the primary ID would be the unique identifier for each record in that table, while the foreign key would refer to the unique identifier of the parent's primary key. This type of relationship can be created in Salesforce using lookup fields or master-detail fields. This book assumes you already know the differences between the two. You will also cover some particular differences that you need to remember in later chapters:

### One-to-Many

![](img/a3b1eecf3c87c814267f2877e8a8be1a_85_0.png)

Figure 2.13 – One-to-many relationship

- **Many-to-many relationship**: This type of relationship occurs when multiple records from the first table can be associated with multiple records from the second table. This is also a common type of relationship that you come across every day. Think of the students and courses example, where each student can attend multiple courses, and each course can have multiple students attending it.

To create a many-to-many relationship between two tables, you need to create a third table, often called a **bridge table** or a **junction table**. This many-to-many relationship would effectively be broken into two one-to-many relationships. The junction table's records would hold the value of two foreign keys, each pointing to a record from the two main tables.

Junction objects can be created in Salesforce by creating a custom object with two master-detail fields, or with two lookup fields (in this case, it is more often referred to as a bridge rather than a junction). Both approaches are valid and have their most suitable use cases.

You can even create a junction object using a mix of master-detail fields and a lookup field. This is assuming you put all the validation and business rules required in place to ensure they are always populated with the right values. You will come across examples of and use cases for Salesforce junction objects in later chapters:

### Many-to-Many

![](img/a3b1eecf3c87c814267f2877e8a8be1a_85_1.png)

Figure 2.14 – Many-to-many relationship

Keep in mind that the cardinality of these relationships can take different forms. For example, a one-to-many relationship might be better represented as one-to-zero-or-many if there are occurrences where the first table will have zero related records from the second table, as illustrated in the students and courses example earlier.

The following diagram illustrates a list of other relationships’ cardinalities. The one-and-only-one relationship could be a bit confusing. It is used when the relationship between two records from two different tables cannot be modified—for example, a user and a login ID. Once a login ID is associated with a user, it cannot be assigned to any other user after that. And it can be associated with one user only. This differs from the ownership relationship between a user and a mobile phone, for example, where the relationship itself might get updated in the future if the phone were to be sold to another owner:

| Term | Symbol | Term | Symbol |
|---|---|---|---|
| One | + | Many | < |
| Zero or one | O+ | One or many | <| |
| One and only one | +|| | Zero or many | O< |

Figure 2.15 – Table relationship cardinalities

### Summary

In this chapter, you learned some historical information about database design, as well as how a proper design can be crucial to the overall Salesforce solution. You also learned about some key concepts, such as reference data, reporting data, and big data, in addition to general information about document management systems.

You then covered various data modeling principles and activities, including the process of normalizing and denormalizing data. You also explored other common architectural knowledge areas, such as normal forms, database relationships, and cardinality.

All this knowledge will be required when you are designing your secure and scalable Salesforce solution. In the next chapter, you will acquire another set of essential common architectural skills. This time, you will focus on the ever-growing and always-interesting integration domain.

### Chapter Review Flashcards

Before you proceed to the next chapter, it is recommended that you go through the flashcards from this chapter first. These flashcards condense all the chapter concepts into smaller and easily manageable chunks that will help you with quick review and retention. By engaging with these flashcards, you will strengthen your understanding of key topics, identify areas that require further study, and build your confidence before moving on to new concepts.

The following image shows an example of the flashcards interface.

Dashboard > Chapter 2 Flashcards

### Chapter 2 Flashcards

![](img/a3b1eecf3c87c814267f2877e8a8be1a_87_0.png)

Figure 2.16 – CTA flashcards

1. Open your web browser and go to https://packt.link/ctach2. You will see the following screen:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_88_0.png)

Figure 2.17 – Chapter summary and login

You can also scan the following QR code to access the website:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_88_1.png)

Figure 2.18 – QR code to access Chapter 2 flashcards

2. Log in to your account using your credentials. If you haven't activated your account yet, refer to Instructions for Unlocking the Online Content in the Preface for detailed instructions. After a successful login, you will see the following screen:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_89_0.png)

Figure 2.19 – Chapter summary and end-of-chapter flashcards

3. Click Start on a flashcards stack to begin your review.

## Core Architectural Concepts: Integration and Cryptography

This Book Comes with Free Online Content

With this book, you get unlimited access to web-based CTA exam prep tools like flashcards and exam tips.

![](img/a3b1eecf3c87c814267f2877e8a8be1a_91_0.png)

Figure 3.1 – CTA online resources dashboard

To unlock the content, you'll need to create an account using your unique sign-up code provided with this book. Refer to the *Instructions for Unlocking the Online Content* section in the *Preface* on how to do that.

