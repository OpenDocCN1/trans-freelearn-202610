Of course there may be processes where we aim for such behavior, but usually, when people think about parallel process paths, they tend to expect a situation where we must wait until all the parallel parts are done before we move on. This behavior is extremely useful from a business point of view because it shows us that we can assemble a device only when we have all the parts ready, or that we can only prepare an offer to a customer when we have both the business part and the financial part ready. So, how can we get this behavior?

The solution to our problem is the concept of synchronization. Unlike exclusive gateways, where the use of a merging gateway was optional, for parallel gateways, we’ll often want to insert a parallel merging gateway as well.

Let’s see what inserting such a gateway will change in our example. For simplicity’s sake, I’m no longer inserting task F, which was previously intended to show what happens when tokens flow through the process uncontrolled. This time, we’re no longer assuming how long tasks take or what the availability of resources is because we want to focus on how synchronization using a merging parallel gateway looks like in practice.

![](bbox=[0.1328, 0.4004, 0.8691, 0.625])

Figure 8.18 – Synchronization with a merging AND gateway

The beginning is familiar: after task A is completed, the token arrives at the AND gateway. At this gateway, it’s multiplied (we can jokingly say that the token is photocopied) so that each of the sequence flows leaving the gateway gets its token. Then, each token flows independently and activates tasks on the paths. The availability of resources determines the order in which they’re performed. Thus, the ABCDE, ABDCE, and ACBDE orders are possible.

But how do we know that we can already perform task E? And why do we perform it once? It’s all due to the merging parallel gateway, which provides synchronization. The moment the first token reaches the synchronizing gateway, it’s stopped because the gateway waits for tokens from all sequence flows that enter it. In our case, only when the third token arrives at the gateway will the gateway merge all of them into one and allow it to pass.