

## Further reading

If you want to learn more about the modeling notations covered in this chapter (and more), I suggest you check out the following books and resources:

- If you would like to learn more about the history of BPMN and its relationship with other standards, such as XPDL, I would recommend a very interesting free paper, _Business Process Model and Notation—BPMN_, by Mark von Rosing, Stephen White, Fred Cummins, and Henk de Man, available at `https://www.omg.org/news/whitepapers/Business_Process_Model_and_Notation.pdf`. You can also read the full book, _The Complete Business Process Handbook: Body of Knowledge from Process Modeling to BPM, Volume 1_, by Mark von Rosing, Henrik von Scheel, and August-Wilhelm Scheer.

- If UML is something you would like to explore further, I would suggest checking out this resource from my go-to expert in UML Filip Stachecki. Visit `https://www.uml-diagrams.org/` for a great overview of what UML has to offer.

- If you prefer learning from books, on `https://modeling-languages.com/list-uml-books/`, you will find a list of many great titles. This website offers many useful resources about UML modeling (and more) as well.

- Finally, I would recommend `https://agilemodeling.com/` by Scott Ambler.

- For more resources, visit the GitHub repository for this book: `https://github.com/PacktPublishing/Practical-Business-Process-Modeling-and-Analysis/tree/main/Chapter07`. You will also find information about notations such as VSM or UPN that we did not cover in this chapter.

**Unlock this book’s exclusive benefits now**

Scan this QR code or go to packtpub.com/unlock, then search this book by name.

Note: Keep your purchase invoice ready before you start.

![](img/a4c90ee3d142c33743706d37c65779c2_215_0.png)

## BPMN – What You Need to Know

In Chapter 7, we briefly covered the history of process modeling notations and learned about the most important shapes of BPMN. BPMN is a very logical and easy-to-learn process modeling notation if you know how to approach it. In this chapter, we’ll cover its basics, including both its most crucial elements – that is, objects that can be used in a model (events, activities, gateways, swimlanes, artifacts, and data objects) and fundamental concepts that are important for having a proper understanding of BPMN diagrams.

In this chapter, we’re going to cover the following main topics:

- How to read a BPMN diagram
- When and how to use gateways in BPMN
- How start and end events can be used
- How to show work in a process diagram with tasks and sub-processes
- How to add more meaning to your BPMN diagram with additional elements, such as swimlanes, artifacts, and data objects

> **Note**
> This chapter is authored by Zbigniew Misiak.

### How to read a BPMN diagram

Let’s go back to the complaint handling process you know well from Chapter 7. This time, we’ll spend some more time discussing the basics of BPMN and explaining the usage of the key elements of the notation based on this example.

The following is a horizontal version of the BPMN diagram. While BPMN allows both horizontal and vertical orientation of diagrams, the horizontal orientation is much more common:

![](img/a4c90ee3d142c33743706d37c65779c2_218_0.png)

Figure 8.1 – Complaint handling process in BPMN

The following two figures (Figure 8.2 and Figure 8.3) will help you learn more about the most important shapes you can see in Figure 8.1:

![](img/a4c90ee3d142c33743706d37c65779c2_219_0.png)

Figure 8.2 – Basic elements of BPMN (part 1)

### Pools and lanes

Let’s begin by looking at the two large rectangles that are visible in _Figure 8.1_: **Company ABC** and **Customer**. In BPMN, we call them **pools**. This name is meant to suggest that the process flows inside the pool (just like people swim inside a swimming pool). This is a very important concept that we’ll develop further in a moment.

As you can see, the diagram can contain several pools if you want to show the different participants involved in the collaboration, each with its own process. The pool can contain lots of elements inside, describing the process in more detail (this is visible for element **1a** in _Figure 8.2_), or just representing that there’s such a participant, without specifying how it performs its process (this, in turn, is option **1b**).

For **Company ABC**, we can see not only elements representing what happens in the process but also some kind of structure showing us who performs what steps. Modeling **lanes** (see element **1c**) are used for this purpose.

It’s worth noting that the name **lane** is another reference to a swimming pool; it can be divided into lanes to organize the way people swim. In our case, we have a lane representing the **complaints handling team**, an additional one for the **finance team**, and finally one for the **logistics team**.

The second pool, representing the customer, doesn’t contain any visible content. Such a pool is often called a **collapsed pool** or **black-box pool**. This suggests that from the point of view of the diagram, we either don’t know what the performer does or we don’t need to describe it in this diagram. Just in case you’re concerned about missing some important insights regarding customer activities, for example, the idea is to make a diagram more readable by hiding unnecessary details. However, they can be captured in another connected diagram.

As you’ll see later in this chapter, pools (and lanes) are great not only for showing the responsibilities in a process but also for showing what the cooperation between different entities involved in a process looks like. Pools and lanes are part of the BPMN category called **swimlanes**.

### Events

While pools and lanes allow us to understand who does what in a process, we still need to understand the scope of the process, especially what makes the process start and how it ends. For this purpose, BPMN offers us events – elements that are always shown as circles. In our example diagram (**Figure 8.1**), we have two types of events: a **start event** (**2a**) and an **end event** (**2b**), which have a thin single border and a thick single border, respectively.

BPMN also allows you to create slightly more complex intermediate events, something that will be covered in the following chapter (**Chapter 9, Advanced BPMN**). However, all the events represent some state of reality that’s important for the process. Unsurprisingly, start events represent situations that cause our process to start. End events, in turn, show the results of the process, its products, or final states (positive or negative). In our example, we have one start event, **complaint received**, and three end events: **complaint rejected**, **money refunded**, and **product delivered**. This allows us to see the scope of the process right away, even without knowing the individual steps of the process.

### Activities

Now that we know how to represent the scope of the process, it’s time to cover an equally important aspect: how to represent work that needs to be carried out in a process. Those steps representing work (called **activities** in BPMN) can be described as **tasks** and **sub-processes** and are shown as elements 3 and 4, respectively. Without them, modeling a process diagram would be nearly pointless.

Tasks and sub-processes are always shown as rectangles with rounded corners. Tasks represent simple, indivisible process steps that we don’t need to decompose further for a more detailed description. Sub-processes, on the other hand, represent more complex process steps. These steps could be described in a more detailed model (this approach allows us to navigate from the main model to the lower-level diagrams) or simply serve as placeholders showing places where such decomposition could potentially take place.

Often, sub-processes are useful when we have many diverse things (tasks) to do, and they’re carried out by different roles in the organization. Sub-processes are usually marked at the bottom with a + symbol, indicating that they may hide **something more** than ordinary tasks.

We can see the division between simple and more complex elements in our example, where steps such as **Check complaint validity** and **Order refund** are tasks. On the other hand, more complex steps such as **Refund processing** and **Product shipping** are sub-processes because we could probably define at least several tasks for each of them, most likely being something more than a sequence of activities (for example, in the case of a payment refund, we perform it differently when the customer makes the payment by bank transfer and differently when the payment is made by credit card).

### Gateways

As we just mentioned, processes are rarely simple sequences of consecutive activities. Therefore, to be able to separate a process into different paths (splitting) and to connect these paths (merging), we must use another type of object in the BPMN: **gateways**. Gateways (see element 5) are always represented as diamonds (rhombuses) with incoming and outgoing arrows representing process sequence flows.

In our example, the simplest type of gateway is used – that is, an **exclusive gateway**. Exclusive gateways represent places in the process where we have two or more paths available, of which only one can be selected. In the case of the **Complaint valid?** gateway, we have **Yes** and **No** as options, while for the **Selected option?** gateway, there are two mutually exclusive options for handling complaints: **Refund** and **Replacement** (of course, in practice, we would have more paths presenting other possible ways to handle a complaint, but this would make our diagram a bit harder to read).

