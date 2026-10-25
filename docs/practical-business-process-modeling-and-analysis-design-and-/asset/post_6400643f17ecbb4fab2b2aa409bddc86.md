

## Process Modeling Notations

Chapter 1 provided a great overview of the main reasons for graphical modeling. Process management has a long history, so it should be no surprise that during this time, many ways of modeling processes – so-called modeling notations – emerged. As we discovered in Chapter 6, process modeling can be used on various levels. Models can also be aimed at different audiences.

Because there is no single notation that is sufficient for all users and all purposes, in this chapter, we will learn how to model processes and discuss the differences between those notations.

Since there are dozens of process modeling notations, we will not be able to cover all of them in depth. We will instead go through the history of process modeling, discussing the important notations that are still relevant today, and briefly mention those that are not very popular currently but had an impact on the most popular process modeling notation today – BPMN.

In this chapter, we’re going to cover the following main topics:

- The basics of process modeling
- Going beyond flowcharts – an overview of other notations
- BPMN – how we got there and why it is relevant to us

> **Note**  
> This chapter is authored by Zbigniew Misiak.

### The basics of process modeling

It’s hard to imagine, but the modeling of business processes can be traced back over 100 years. In 1921, Frank and Lillian Gilbreth (brilliant industrial engineers and efficiency experts) proposed a new way of presenting processes in a presentation for the American Society of Mechanical Engineers (ASME) titled *Process Charts: First Steps in Finding the One Best Way to do Work*.

What is fascinating to me is that the article starts with a point that is still valid today:

> "The process chart is a device for visualizing a process as a means of improving it. Every detail of a process is more or less affected by every other detail; therefore, the entire process must be presented in such form that it can be visualized all at once before any changes are made in any of its subdivisions".

Apart from suggesting an approach for process modeling, they also proposed several dozen standard symbols (shapes) for the process charts. Their ideas gained traction, and in 1947, process charts became an ASME standard (in a simplified form with fewer shapes, which was supposed to be less prone to errors).

ASME process charts used shapes such as circles, arrows, squares, and triangles. This way of modeling processes was very simple, and this was done on purpose. Those diagrams were meant to be used not only by managers but also by the people performing the process so that they could understand the process better, find errors, and propose a more streamlined version of the process.

A great quote (which I found in the article named *People Come First* on: http://www.worksimp.com/articles/keynoteworkflowcanada.htm) from Alan Mogensen, who used this way of modeling in consulting projects, shows clearly how process improvement was considered a democratic activity, not restricted to managers or experts:

> "The person doing the job knows more about it than anyone else in the world and is therefore the one person best fitted to improve it."

Most likely, you have seen some flowcharts already. However, if you decide to check the image showing the original flowchart shapes, which is available on the GitHub page with resources for this chapter, (https://github.com/PacktPublishing/Practical-Business-Process-Modeling-and-Analysis/tree/main/Chapter07) you may not be familiar with all the shapes. That’s because flowcharts kept evolving, especially after it was discovered that they are very useful for creating diagrams representing computer code.

There are several standards specifying shapes used in flowcharts, for example, ANSI’s *Flowchart Symbols and their usage in information processing* from 1970 and ISO 5807 from 1985: *Information processing – Documentation symbols and conventions for data, program and system flowcharts, program network charts and system resources charts*.

However, still, in various programs, you may encounter some differences. Since we will be comparing different notations, we will use a common style of table that will allow you to see similarities and differences more easily. In this table, you will see an overview of objects (shapes) frequently used in diagrams, but please note that they also need some connectors (arrows). Each notation uses its own style of connectors, but as you will see, connectors showing how the process flows are usually drawn with solid lines.

| Modeling concepts | Shape and name | Description |
|---|---|---|
| Start of the process diagram | Start | First element of a process diagram. Start and end shapes look the same. |
| Process step (simple) | Process | A simple process step representing some work to be done in a process. |
| Process step (complex) | Subprocess | A more complex step of a process, which can have several sub-steps – often shown in a separate diagram. |
| Process split | Decision | The place where a process splits into two or more paths on the basis of some decision. |
| End of a process diagram | End | The last element of a process diagram. Start and end shapes look the same. |

Table 7.1 – Flowchart shapes (basic)

As you may imagine, the shapes shown in the preceding table are very useful and allow you to document many interesting aspects of processes, but they are not everything that flowcharts have to offer. The following table shows some less frequently used modeling concepts:

| Modeling concepts | Shape and name | Description |
|---|---|---|
| Document creation | Document | Step for creating a document |

| Modeling concepts | Shape and name | Description |
|---|---|---|
| Handling inputs or outputs | Data or input/output | Data or materials entering the process from the outside, or the output of the process |
| Navigation – the same page | On-page reference | Jump to another part of the flowchart |
| Navigation – another page | Off-page reference | Hyperlink to another page of the flowchart |

Table 7.2 – Flowchart shapes (more advanced)

Now, let’s see in practice how a business process could be documented as a flowchart. We will continue using the process of complaint handling of a B2C company that you saw in the previous two chapters.

The process is initiated by a communication from a customer who reports a complaint (via email, phone, or chat on the website). The complaint is checked by a complaint specialist for validity (within a system that automates this entire process). If the complaint is invalid, the customer receives a message about its rejection, with a justification (the justification should have been prepared by the specialist in the previous step). If the complaint is valid, the customer receives information about approval of the complaint, and then the specialist determines which solution will be satisfactory to the customer: refund (part of) the payment or replace the product with a new one. If the customer opts for a refund, the corresponding order goes to the financial team to make the refund. If the customer prefers a replacement, the order goes to the logistics team to ship the appropriate product. The process can thus end in three ways:

1. The customer may receive a message about the rejection of the complaint (along with a justification).
2. The customer may receive a monetary refund.
3. The customer may receive a replacement product.

