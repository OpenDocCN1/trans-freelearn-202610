In practice, both diagrams in Figure 8.22 have the same meaning. The diagram on the left – with an AND gateway – takes up a bit more space, but you can immediately see how to interpret the model. In the case of the diagram on the right, you need to take a closer look at the process flows to make sure they don’t have conditions and that each path gets its token:

![](bbox=[0.1328, 0.1914, 0.8672, 0.3398])

Figure 8.22 – Comparison – splitting process paths with and without an AND gateway

As we mentioned earlier, while it’s possible to split paths without gateways, in most cases, skipping the synchronization gateway isn’t a good idea. For example, Figure 8.23 shows a situation in which nothing synchronizes the three tokens that were created in task A, which of course results in task E being executed three times.

As we mentioned for Figure 8.17, such a way of modeling is formally allowed, but you must be aware that many people will consider it a mistake and even more people will misread our intention, so this isn’t recommended.

![](bbox=[0.1328, 0.5586, 0.8672, 0.7773])

Figure 8.23 – A risky way of modeling – lack of synchronization

It’s also possible to avoid using gateways when there are multiple flows with conditions (that is, the situations we discussed with XOR and OR gateways). In this case, a new type of sequence flow is needed – a conditional sequence flow. A conditional flow can come out of a task or sub-process, but