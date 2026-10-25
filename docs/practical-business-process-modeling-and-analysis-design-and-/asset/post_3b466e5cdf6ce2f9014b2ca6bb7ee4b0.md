

At this point, it’s worth noting that sending messages by end events makes sense in the case of highly automated processes, where such sending doesn’t require human intervention. That’s why we usually send messages with tasks, and the end event is only used to summarize what’s already happened. This can be seen in Figure 8.39.

![](img/a4c90ee3d142c33743706d37c65779c2_261_0.png)

Figure 8.39 – Part of the example process – sending messages with tasks and proper naming of events

However, if we wanted to (and our process was properly automated), we could send a message from the end event, as shown in Figure 8.40:

![](img/a4c90ee3d142c33743706d37c65779c2_262_0.png)

Figure 8.40 – Part of the example process – sending a message with the end event

Figures 8.39 and 8.40 are similar in meaning but there are still some differences. In the former figure, message flows are connected to the send task and the end event only states the result that refers to work that happened within the previous task.

In the latter figure, the task only _prepares the ground_ for sending the message because the user enters the reason for rejection. This will be sent to the customer in the next step (in case you’re curious, for Figure 8.39, this preparation should happen in the **Check complaint validity** task because the send task shouldn’t be used for writing and sending emails, only for sending). The message is sent by the **Complaint rejected** end event.

If you use end events that send a message, it’s worth taking care not to mistakenly suggest that messages are sent twice – once by a task and additionally by an end event, as seen in the anti-pattern shown in Figure 8.41. This kind of confusion is particularly dangerous when you don’t use message flows, which clarify the communication with external participants:

![](img/a4c90ee3d142c33743706d37c65779c2_263_0.png)

Figure 8.41 – Anti-pattern – sending a message twice with a task and an end event

## Timer events

Now that we’ve learned about more advanced rules for modeling start and end events while using messages as examples, it’s time to become familiar with an additional type of trigger – time. Since humans can’t control time, this type of trigger only exists in a catching form, so there’s no such thing as a timer end event.

Let’s learn more about the timer start event. These events have a very distinctive appearance since their icon is a clock face. They’re used to show that the process starts at a specific moment. This can be a specific moment (for example “The first business day of the quarter,” “6 P.M. on weekdays,” or “December 31, 11:59 P.M.”) or a period (for example, “Every 8 hours”). With these types of events, we can easily represent time in our processes.

![](img/a4c90ee3d142c33743706d37c65779c2_264_0.png)

Figure 8.42 – Timer start event

Let's return to our complaint handling example. When discussing _Figure 8.31_, we said that for the related process where we analyze reasons for complaints to improve our processes, we could use the **Summary of reasons of complaints needed** start event.

This would show that the process of analyzing the reasons for complaints is done on an irregular basis, depending on the expert decision of those involved in the process. However, if we wanted to ensure some regularity as part of organizational improvement, we could decide that this process is performed regularly every month, and we always perform it on the first working day of the month. _Figure 8.43_ shows what this could look like.

![](img/a4c90ee3d142c33743706d37c65779c2_264_1.png)

Figure 8.43 – Example – timer start event in practice

Similarly, a timer start event could be used within the shipment of goods to group a batch of packages and send them together once a day, for example, or within the process of refunding customers who have paid by wire transfer so that our employees from the finance team don't have to log in to e-banking every time a refund is needed. Instead, they gather a batch of transfers to be made and send them together at the right time.

In this section, we learned how start and end events allow us to convey very important information about what can start our process and how it can end. Thus, they define the scope of our process. But what can we do if, while modeling, we notice that there are so many process steps that our diagram is no longer readable? We'll talk about this in the next section.

## Improving diagram readability with sub-processes

As we already know, a very important issue in creating models is to ensure that they can communicate with others. For this to happen, our models must be readable by the audience. Readability depends on many aspects, but some of the most important are the size of the model (how much space it takes up and how many objects it contains) and how easy it is to read.

At this point, it’s worth noting that it’s often difficult to convey good modeling practices in a book due to the nature of the medium. That’s why some of the diagrams in this book are only available on its GitHub repository.

In practice, we usually create models on a computer and read them either electronically or as a hard copy. A paper book is much smaller than a standard printout, so larger diagrams need to be rotated and placed on separate pages; if they’re placed in the text, there are suggestions to squeeze everything to make it look better. This often leads to snake diagrams, which is an anti-pattern.

While the BPMN specification itself doesn’t place restrictions on us – a diagram containing one start event, one task, and one end event is just as correct as a diagram consisting of several hundred tasks and dozens of gateways – the size of the diagram is very important to its readers.

There’s no consensus among modeling experts on how large a diagram can be to maintain readability. It’s often argued that the ideal diagram should fit into a single sheet of paper.

I recommend creating diagrams that have no more than 20 to 30 elements. But what if we have a very complex process that needs 40, 50, or more elements to cover everything with a proper level of detail? This is when sub-processes come in handy.

Sub-processes allow modeling to occur hierarchically. While the BPMN doesn’t support the high-level process architecture, it does allow us to move between different diagrams, from general to specific, in a very convenient way.

Let’s look at Figure 8.44, which shows our example process for handling a complaint. To be more precise, we’re looking at the part that documents what happens when the chosen complaint handling option is a refund.

We can see that an employee from the claims handling team needs to order a refund. Afterward, this refund is processed by the finance team.

Figure 8.44 – Example process with visible sub-processes allowing us to navigate to lower-level diagrams

## Embedded sub-process

The element we can see here is a standard type of sub-process known as the **embedded sub-process**. This often comes in a collapsed form and is shown as a rectangle with rounded corners, with a plus marker at the bottom.

![](img/a4c90ee3d142c33743706d37c65779c2_267_0.png)

Figure 8.45 – Embedded sub-process

Of course processing refunds is quite a complex matter. Local regulations may differ just like company policies, but we can assume that if a payment needs to be refunded, the refund should be made using the same method that the customer picked.

Therefore, we should support not only card payments, but also e-wallets/payment systems, bank transfers, and possibly something more.

Are those details relevant to someone interested in the complaint handling process? Should we add them to the complaint handling process diagram? For most audiences, this would probably be too many details. So, instead of helping our readers, it would hinder communication. Therefore, according to the Goldilocks principle, it’s better to show only as much as necessary.

How we could do this in practice? Instead of adding everything to the main diagram of the complaint handling process, we can create a new diagram showing how we process a refund and link it with the corresponding sub-process object in our main diagram. This hierarchical modeling makes it easy to move to a lower level with a single click, facilitating work with connected diagrams.

