# # Embedded sub-process

The element we can see here is a standard type of sub-process known as the **embedded sub-process**. This often comes in a collapsed form and is shown as a rectangle with rounded corners, with a plus marker at the bottom.

![](bbox=[0.3789, 0.207, 0.5879, 0.291])

Figure 8.45 – Embedded sub-process

Of course processing refunds is quite a complex matter. Local regulations may differ just like company policies, but we can assume that if a payment needs to be refunded, the refund should be made using the same method that the customer picked.

Therefore, we should support not only card payments, but also e-wallets/payment systems, bank transfers, and possibly something more.

Are those details relevant to someone interested in the complaint handling process? Should we add them to the complaint handling process diagram? For most audiences, this would probably be too many details. So, instead of helping our readers, it would hinder communication. Therefore, according to the Goldilocks principle, it’s better to show only as much as necessary.

How we could do this in practice? Instead of adding everything to the main diagram of the complaint handling process, we can create a new diagram showing how we process a refund and link it with the corresponding sub-process object in our main diagram. This hierarchical modeling makes it easy to move to a lower level with a single click, facilitating work with connected diagrams.