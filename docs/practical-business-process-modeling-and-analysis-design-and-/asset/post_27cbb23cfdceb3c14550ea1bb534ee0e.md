

## Connecting objects

For this purpose, we would need an additional category of BPMN: so-called **connecting objects**, or colloquially speaking, arrows. The most important type of arrow in BPMN is the **sequence flow**, which is always marked by a black solid line with a full triangular arrowhead at the end (shown as element **A** in _Figure 8.3_).

As the name implies, this kind of arrow shows us in which sequence elements are activated as it connects events, gateways, and activities within a pool so that it’s always clear what comes before and what comes after the given element in the process. The sequence flow shows us the control we have over the elements of the process – either implemented by the process automation tool or by the process owner, who controls what happens in the process.

Usually, the sequence flow is just a line without additional markers or texts but if it’s used where the process paths split (most commonly after the gateway), you’ll often see textual labels on the arrows that help you figure out which path represents which flow (in our example, it will help you figure out what will happen if our process needs to support the **Refund** option, and what will need to be done for the **Replacement** option).

# How to read a BPMN diagram

To summarize our principle regarding the process flow, we can say that a process always flows within a specific pool and can move between different lanes: from the start event through various activities and gateways to the end event, always following the sequence flow arrow (no gaps or jumps).

Since a sequence flow can only occur within a single pool, how do we show that different entities work together? This is where an additional arrow type – message flow, labeled as element B – comes in handy.

Unlike the sequence flow, we can only use it between pools. The message flow is always marked with a dashed line. At its beginning is a circle filled with white, and at the end is a triangular empty arrowhead.

A different type of line might suggest that this type of flow behaves differently from a sequence flow, and indeed this is the case. While a sequence flow tells us about control and suggests that a process has a defined course that can be enforced one way or another, a message flow shows that different entities exchange messages, or information, with each other. However, those exchanges are more like requests, proposals, or suggestions to another entity. A customer has the right to make a complaint but can’t force an organization to acknowledge it if it’s unfounded.

The elements that we’ve discussed so far allow us to describe the logic of the process and show how it interacts with other participants. With this knowledge, you can now read the vast majority of BPMN diagrams. However, apart from those key building blocks of the notation, there are also other categories of objects that give us more options in two additional areas – conveying additional information about the process and describing how to use data in the process.

# Artifacts

To provide additional information about the diagram for the people reading it, so-called artifacts are used in BPMN. There are two types of artifacts: text annotation and group. These are visible as elements 6a and 6b in Figure 8.3, respectively.

These objects allow process modelers creating AS-IS or TO-BE diagrams to capture useful comments relating to how the process currently works or what we want to change about it. However, note that artifacts don’t change the meaning of the diagram, which is based on the elements that we discussed previously. So, for diagrams serving as documentation and for those used for automation, artifacts only provide more information without influencing the core message that’s captured in a process diagram.

A text annotation is an element that allows you to place some text in the diagram. It has a very characteristic shape of a left square bracket with text inside. Text annotations can be placed anywhere inside a process diagram or connected with a particular diagram element. The former option makes sense if your comment is relevant to the whole process. The latter option is useful if we want to show that the comment applies to some specific diagram part. In this case, an additional type of arrow – an association drawn with a dotted line (element C) – is used. In our example, text annotation is visible next to the start event and shows the following text: Email, phone, or chat.

A group, in turn, allows us to visually distinguish certain elements of the diagram to emphasize that they have something in common and belong to some specific category. A group is shown as a rectangle with rounded corners, drawn with a line composed of dots and dashes (which reminds me of Morse code). The group isn’t connected to other elements of the diagram by arrows, so it’s easy to distinguish it from (nearly) all other elements of the notation. Groups can have labels that show what the contained elements have in common.

The last category of BPMN objects is **data objects**, which (not surprisingly) allow us to show how data flows in the process. There are several types of data objects with different visualizations, but usually, they look like a sheet of paper with the top-right corner folded (in our example diagram shown in Figure 8.1 it’s **complaint documentation** – that is, element 7a from Figure 8.3).

In our example (Figure 8.1), we can also see a specific type of data object (**CRM system**) that looks like a cylinder with three stripes at the top (element 7b). To show how data flows from or to an activity in a BPMN diagram, we can use the **data association** arrow. This looks very similar to a plain association, but it has an arrowhead drawn with a black line (element D).

As shown in Figure 8.1, objects in BPMN can differ within each type. For example, tasks may have icons in the top-left corner indicating what type they represent; inside the events, there may be special icons indicating how the process starts (for example, our start event has an envelope icon inside, which means that the process was triggered by a message from the customer); and sub-processes may differ, for example, in the style of the border. This way, BPMN allows us to convey additional information about the process. We’ll cover those shape differences in this and the following chapter.

Overall, if we tried to count all the possible combinations of allowed shapes, icons, and line styles, there would be over a hundred shapes that have a specific meaning. Luckily for us, not all these elements are equally popular. Therefore, in this book, we’ll start with the most useful and popular elements and devote the most attention to them. For those that are less frequently used, we’ll allocate less space, but we’ll point out when and for whom they could be useful.

# How does the process flow?

As we know from the previous section, one of the more important issues is to understand what work needs to be done in a process and what the relationships are between the various elements, or, more simply, what needs to be done and when.

It seems trivial, but for someone who doesn’t know the whole process well, the mere information about what is to be done won’t be enough. It’s like giving a person learning how to cook a recipe for a dish but without information on when to add which ingredients. The mere fact that we’ll perform all the necessary steps isn’t enough – we can’t perform them in the wrong order or do something that wasn’t needed in that situation.

Process models can be small or large, simple or very complex, but regardless of their size and complexity, when we analyze them, we can distinguish certain recurring patterns. The concept of workflow/ process **patterns**, which were created for analyzing process automation (workflow) solutions, proves to be very useful in analyzing processes and learning modeling notations too. That’s why we’ll be using them in this book.

While discussing various process patterns, we’ll be using an additional concept since it’s part of BPMN specification – the **token**. This isn’t one of the notation elements that can be used when drawing a BPMN diagram since it’s only a theoretical concept. However, it’s very useful for understanding how a process is executed, especially when there are several paths within a process. In case you’re curious, there are BPM tools that allow you to use tokens in practice – either for simulating business processes or for visualizing the progress of work in a process. However, BPMN specification doesn’t require tokens to be implemented in modeling or automation tools.

A token is born when a start event is triggered and then flows through the process along the sequence flow arrow until it reaches the end event, where it’s consumed.

# ## Sequence

The simplest possible pattern is a **sequence** – a series of actions that are performed in succession. This sequence shows what should happen first so that it’s possible to move to the next stage of the process. To discuss this sequence, we’ll once again refer to the concept of a token.

Imagine that we’re building a house. First, we must prepare the foundation. Only then can the walls be put up. Once the walls are standing, we can think about the roof. Common sense tells us that the idea of speeding up construction by putting up walls on an unfinished foundation and covering this with a roof is risky.

Let’s look at an example of such a simple sequence:

![](img/a4c90ee3d142c33743706d37c65779c2_225_0.png)

Figure 8.4 – Sequence

When the process is started, the token goes to task A and makes it activated. This is a formal name from specification In practice, this doesn’t necessarily mean that someone has already started working on task A (for that, you still need a resource, such as an employee assigned to that task), but that if all the circumstances are favorable, you can start working on task A (as opposed to task B, which isn’t feasible yet).

