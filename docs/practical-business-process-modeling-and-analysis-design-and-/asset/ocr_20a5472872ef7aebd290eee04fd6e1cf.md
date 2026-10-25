# # Exclusive gateway (XOR)

In the previous section, we dealt with processes that were a simple sequence of steps. However, usually, our processes are more complex and some fragments are only performed if some condition has been met. The most common way to model such a situation is to use the exclusive gateway. This is often called the XOR gateway (exclusive OR):

![](bbox=[0.293, 0.2219, 0.6797, 0.3328])

Figure 8.6 – Two possible visualizations of the XOR gateway

The XOR gateway can be a simple diamond, or it can have an X marker in the center. Interestingly, while these two visualizations are allowed, they shouldn’t be mixed within a single model. Within the organization, it’s good practice to determine how XOR gateways should be drawn to avoid confusion. I’m in favor of using XOR gateways without the X marker in the middle, but I know that for many people, the X is a great mnemonic to help them remember that it’s an XOR gateway.

# # Naming

The BPMN specification doesn’t impose a specific way of naming exclusive gateways. So, in practice, you can encounter different approaches, as shown in Figure 9.7. In one approach, you don’t name XOR gateways, instead only making sure that the conditions/labels of the arrows coming from the gateway are sufficiently clear (for example, Complaint valid and Complaint invalid).

I prefer the second approach, in which the XOR gateway is named with a question (for example, Complaint valid?), and the answers to this question are placed on the sequence flows coming out of this gateway (for example, Yes and No).