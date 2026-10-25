# # Improving diagram readability with sub-processes

As we already know, a very important issue in creating models is to ensure that they can communicate
with others. For this to happen, our models must be readable by the audience. Readability depends
on many aspects, but some of the most important are the size of the model (how much space it takes
up and how many objects it contains) and how easy it is to read.

At this point, it’s worth noting that it’s often difficult to convey good modeling practices in a book
due to the nature of the medium. That’s why some of the diagrams in this book are only available on
its GitHub repository.

In practice, we usually create models on a computer and read them either electronically or as a hard
copy. A paper book is much smaller than a standard printout, so larger diagrams need to be rotated
and placed on separate pages; if they’re placed in the text, there are suggestions to squeeze everything
to make it look better. This often leads to snake diagrams, which is an anti-pattern.

While the BPMN specification itself doesn’t place restrictions on us – a diagram containing one start
event, one task, and one end event is just as correct as a diagram consisting of several hundred tasks
and dozens of gateways – the size of the diagram is very important to its readers.

There’s no consensus among modeling experts on how large a diagram can be to maintain readability.
It’s often argued that the ideal diagram should fit into a single sheet of paper.

I recommend creating diagrams that have no more than 20 to 30 elements. But what if we have a very
complex process that needs 40, 50, or more elements to cover everything with a proper level of detail?
This is when sub-processes come in handy.

Sub-processes allow modeling to occur hierarchically. While the BPMN doesn’t support the high-level
process architecture, it does allow us to move between different diagrams, from general to specific,
in a very convenient way.

Let’s look at Figure 8.44, which shows our example process for handling a complaint. To be more
precise, we’re looking at the part that documents what happens when the chosen complaint handling
option is a refund.

We can see that an employee from the claims handling team needs to order a refund. Afterward, this
refund is processed by the finance team.