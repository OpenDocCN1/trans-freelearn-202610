![](bbox=[0.13, 0.12, 0.86, 0.43])

*Figure 1.6: Slower order changes reduce inventory oscillations*

As shown in Figure 1.6, the behavior of the system is much better now. The inventory still oscillates, but it starts to settle close to the desired inventory around 20 days after the change in demand.

# Systems surprise us

Systems can react in counterintuitive ways. In this relatively simple system, the store manager was reacting too quickly. Our natural inclination was toward faster perceptions and responses, but they made the system behave even worse. It was only when we tried something that seemed counterintuitive (slower responses) that the behavior of the system improved. This is also true of software systems, and we’ll see some practical examples later in this chapter. Changing some variables can have a big impact on the overall behavior, but others might have little to no effect. In these examples, we saw that a perception delay had little effect, but a response delay did.

Real systems are always more complex than models and can have hidden variables or feedback loops. This system was relatively simple. A more complex system can have multiple stocks and more feedback loops. Systems are inherently oscillatory. System behavior is revealed as a series of events over time. Our human minds are not good at understanding non-linear growth and interactions between elements. We make assumptions about what the current bottleneck is and try to improve elements of the system that will not increase performance unless the limiting factor is improved.