A good example would be sending correspondence in the form of letters. If we do this ourselves, then we know what to send, how to send it, and to whom. However, if we need to send correspondence to hundreds of clients and we use the help of office clerks, it isn’t enough to ask them casually to send mailings.

We need to clearly communicate what is to be sent (a letter template or all the required data), specify the method of sending (an ordinary letter, an express letter, or maybe a letter with proof of delivery), and provide addresses. We can also expect to receive a summary containing information regarding letters that were delivered successfully and those with invalid addresses.

In Figure 8.46, we had two end events showing how our refund could be processed. However, we didn’t cover the case of a failed refund. In practice, however, our sub-process may have several possible outcomes, and not all of them are equally good for us. In such a situation, we often insert an exclusive gateway after the sub-process to show how to deal with the different outcomes of our sub-process.

It’s worth knowing that sub-processes aren’t only used for hierarchical modeling. It may also happen that the entire content of a lower-level process is contained in a sub-process object, which is a kind of container within which its content is shown. Such an approach is called inline modeling. This way of modeling sub-processes in an expanded form isn’t usually recommended and generally makes sense when we use more advanced ways of modeling, allowing us to describe exception handling by boundary intermediate events (something we’ll talk about in Chapter 9).

The following illustrations (Figures 8.49, 8.50, and 8.51) show an example of hierarchical modeling contrasted with inline modeling:

![](bbox=[0.131, 0.529, 0.869, 0.718])

Figure 8.49 – Example of hierarchical modeling – moving to a lower- level diagram through an embedded sub-process

It’s worth noting that the expanded sub-process has no plus marker at the bottom and looks confusingly similar to the task, but most importantly, the sequence flow arrow coming out of task D reaches the edge of the embedded sub-process but doesn’t cross it.