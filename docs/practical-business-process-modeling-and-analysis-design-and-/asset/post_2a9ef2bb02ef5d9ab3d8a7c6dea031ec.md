

These two approaches translate into different ways of naming pools. Often, a pool bears the name of a participant it represents. However, it’s quite common – especially for models created for process automation on platforms that have this requirement – that the pool has the same name as the process that flows in it. A similar approach can also be found in simple modeling tools that have no other place to enter the name of the process being described.

As we have already seen in our example diagram, BPMN allows us to describe a pool with content, but also to show that there’s a process participant with whom we exchange messages, without showing the detailed process of this participant.

In this case, we use a collapsed (black-box) pool, whose contents are not shown. Such a pool has no line separating the name from the content (because there is none), and its name is usually shown in the middle. Please note that you could also add a normal pool and leave it empty.

![](img/a4c90ee3d142c33743706d37c65779c2_275_0.png)

Figure 8.53 – Collapsed (black-box) pool

One of BPMN’s unique features is its support for showing how the process of one participant interacts with other participants. While other process modeling notations allow us to show who’s responsible for what with different kinds of lanes, BPMN allows much more.

## Private processes

The first interesting difference is that in BPMN, processes can be defined as private or public. A **private process** describes, at the appropriate level of detail, what’s going on in the organization being analyzed. They can be used for documentation and analysis purposes (**non-executable** private process model) or process automation purposes (**executable** private process model).

By design, an executable diagram should contain much more technical information to automate the process, such as user forms, technical expressions to control the flow of the process through different paths (depending on the data that’s received), and more.

A diagram that’s used for documentation, on the other hand, can contain only as much detail as what’s needed for the purpose for which it was created. Figure 8.54, which shows a simplified version of our process, shows what a diagram that’s been created as process documentation may look like (it would also likely have descriptions of the process steps, but they’ve been hidden here).

Figure 8.54 – Example process shown as a private process for documentation purposes

## Responsibilities in a process – pools and lanes

Interestingly, from a formal point of view, we don’t always have to insert the pool into our diagram.
The specification allows us to omit it for one selected process. So, once again, it invokes the concept
of invisible default elements.

I don’t recommend this way of saving space because for many audiences, the view of lanes dividing
the invisible pool is highly confusing, and additionally, by getting rid of the lanes, we make it more
difficult to see who’s responsible for which steps within the process.

## Public processes

A **public process**, on the other hand, is used to show only those elements of the process that are relevant
to the communication between the participants (basically, elements related to sending and receiving
messages). A public process is an abridged version of a private process that allows us to quickly show
business partners what they need to know about our way of doing business, without burdening them
with unnecessary information about process steps that don’t interest them.

Figure 8.55 – Example process shown as a public process

## Collaboration diagrams

Let’s go a little further and show how different participants work together using **collaboration diagrams**. These contain two or more pools (expanded or collapsed) between which messages are exchanged.

Often, collaboration looks like the example we know from Figure 8.1 – within the main pool, we have all the necessary process steps, not only those needed for message exchanges. Additional pools can be collapsed or contain their processes inside.

Figure 8.56 – Example process shown as collaboration

If we wanted to show only the communication between pools, we could use a collaboration diagram with all pools collapsed, but this isn’t common.

## Good practices and errors to avoid

Speaking of pools and lanes, it’s also worth remembering three BPMN modeling principles that can help us avoid many common mistakes (examples of which I’ve included in Figure 8.57):

- The process flows within a specific pool and can move between different lanes, from the start event through various activities and gateways to the end event. It always flows along the sequence flow arrow (without “gaps” or “jumps”).
- The sequence flow can only occur within the pool; it can’t cross the border of the pool.
- The message flow can only occur between pools; it can’t take place within a single pool. This also applies to sending and receiving messages between lanes in the same pool.

![](img/a4c90ee3d142c33743706d37c65779c2_282_0.png)

### ANTI-PATTERN

Figure 8.57 – Anti-pattern – a summary of the most common errors related to pools and lanes

## Responsibilities in a process – pools and lanes

The first thing that strikes us is the sequence flow between **Task 1** and **Task A**, which are in two different pools. This is not in accordance with the rules of notation because the sequence flow can’t pass between different pools – it’s always related to a specific pool.

If we take a closer look, we can also see a more subtle error – the process starts with **Start event 1** in **Pool 2** and then continues in **Pool 1**. This is also wrong because **Pool 1** should have its own start event, which would most likely have an incoming message flow coming out of **Task 1**.

The next mistake is that **Lane 1** and **Lane 2** are too independent as they have their own start and end events, while the process should flow through the entirety of **Pool 1**. This would emphasize that the lanes are just divisions of the pool and the process is associated with the pool, not assigned to a specific lane.

Another error that isn’t so easy to catch is that the message flow coming out of **Task B** doesn’t enter **Task 7** in **Pool 2** but is attached to the pool edge. Admittedly, BPMN allows us to start and end message flows at the edge of the pool, but this only makes sense for pools that don’t contain content (usually collapsed pools). If the pool does have content, then we should show precisely where our message flow is leading to or from.

Errors occur in two more places: in **Pool 1**, we can see that **Task B** and **Task C** aren’t connected by the sequence flow. In **Pool 2**, on the other hand, we have a gap in the sequence flow between **Task 1** and **Task 7**. Both cases violate the rule that says that there can be no “gaps” or “jumps” in a sequence flow.

In the case of **Pool 2**, we can also see that the process is incomplete. It has a start event and **Task 1**, but then there’s a break. Later, we can see that **Task 7** and **Task 8** exchange messages with the process contained in **Pool 1**, but we don’t know what comes after **Task 8** because there’s no end event at the end.

This kind of modeling, in which we describe some but not all elements of a process and don’t even bother to connect them properly, isn’t appropriate.

If we don’t need details, then representing the participant with a collapsed pool is enough. If we’re only interested in the elements that are important for communication, we use the public process, which may not contain all the elements (as in our example, where we have **Tasks 1**, **Task 7**, and **Task 8**), but it still needs to be valid. Of course, we could also model the process contained in **Pool 2** in detail.

The last major error – sending messages within a single pool – also occurs in several places. The first time we saw it was between **End event 1** and **Start event 2**. This is a consequence of “slicing” the process into mini processes for each lane: not from the perspective of the whole organization (represented by the pool) but from the perspective of its organizational unit (represented by lanes).

Of course, messages can’t be exchanged between **End event 1** and **Start event 2** because we aren’t allowed to have message flows (or message exchanges) within a single pool.

