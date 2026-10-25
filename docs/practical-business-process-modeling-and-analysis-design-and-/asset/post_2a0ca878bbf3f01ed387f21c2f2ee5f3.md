

From a technical point of view, the user in charge of the **Identify way of complaint handling** task, who’s working in the IT system, would have a radio button on the case handling form where they would select the correct option – **Replacement** or **Refund**. This selection would affect which task would be received upon completion of the current task. It could also work like this in the earlier part of the process, where the user would have to mark whether the complaint is legitimate or not.

![](img/a4c90ee3d142c33743706d37c65779c2_233_0.png)

Figure 8.13 – Part of the example process – gathering data in a task that’s been placed before a gateway

However, it’s also possible to have a situation where the customer is asked to specify the preferred way to resolve the complaint earlier in the process. If we assume that the customer always completes the relevant fields in the form (and doesn’t change their mind afterward), our process will look like this: the data as to the preferred way of resolving the complaint is obtained in the **Receive the form** task and only affects the process in the **Selected option?** gateway. At this point, our employee would have to choose whether they recognize the complaint as valid or not. Whether they’d get the **Order refund** or **Order shipping of a replacement product** task wouldn’t depend on what they _click_ in the system because the data would already be available.

![](img/a4c90ee3d142c33743706d37c65779c2_234_0.png)

Figure 8.14 – Part of the example process – a gateway using data that was gathered in the earlier part of the process

Once we have the data, and the token has reached the gateway, the conditions that were defined for each path of the process are analyzed. In the case of an exclusive gateway, the first path whose condition is met will receive the token and the process will proceed. Naturally, such exclusive gateway behavior makes it very important to define the conditions on the paths properly. Therefore, a very popular approach is to try to document processes in such a way that exclusive gateways have only two outgoing paths, representing Yes and No options for the question asked in the gateway. We saw an example of this approach in Figure 8.14 for the Complaint valid? gateway.

However, BPMN allows us to separate process paths in other ways so that process flows coming out of a gateway have conditions that are more complex than Yes and No. This gives us more options (especially if we have three or more possible flows in our process) and makes the diagrams more readable (otherwise, we might have to insert gateways one after the other), but it also forces us to take more care with well-thought-out conditions.

## Watch out for errors with conditions

Let’s use a slightly more elaborate version of our process to analyze an anti-pattern of how to handle this situation. Let’s assume that our organization has decided to simplify the claims handling process so that claims for low-value goods are automatically accepted so that no employees are involved. Figure 8.15 shows us that complaints with a value of less than 100 USD will be automatically accepted, and a refund will be issued (we assume that this is the condition of our fast track), while complaints of more than 100 USD will be analyzed by an employee.

However, what about complaints worth exactly 100 USD? Unfortunately, our process would encounter an error in such a situation and won’t be able to continue because none of the provided paths supports such an amount. For the reader of the process, this is confusing, but for the automation of the process, it’s an even bigger problem.

![](img/a4c90ee3d142c33743706d37c65779c2_235_0.png)

Figure 8.15 – Anti-pattern – unclear conditions regarding the amount of the claim

It would also be problematic if our conditions overlapped. Such a case, however, would be less troublesome than the previous one, because then you can take the first path for which the condition was met.

## Default sequence flow

But what if we can’t anticipate all the options that might exist? Here, BPMN offers us a solution that’s ingenious in its simplicity – the default sequence flow. We often use it to refer to process flows that are coming out of an exclusive gateway. The default flow has an additional designation that’s visible as a diagonal crossed line near the beginning of the arrow.

![](img/a4c90ee3d142c33743706d37c65779c2_236_0.png)

Figure 8.16 – Default sequence flow

The default sequence flow allows us to indicate which path the process should take if none of the conditions that have been defined on the other arrows are met. This guarantees that our process will never get stuck due to receiving data we didn’t expect.

It’s worth remembering that the default flow doesn’t necessarily mean the most common way to execute the process or the option that’s most expected from the point of view of the organization (the so-called happy path). It’s simply a designation – if none of the defined conditions are met, take that path. IT professionals may associate it with the ELSE command present in many programming languages.

## Parallel gateway (AND)

Another type of data-based gateway is the **parallel gateway**, also referred to as an **AND gateway**. This gateway is denoted by a + symbol in the middle. However, unlike the XOR gateway, this symbol isn’t optional, so in the case of this gateway, the + symbol must always be present.

A parallel gateway is used to mark process paths that must be executed before the process can move on. Interestingly, this doesn’t mean that the activities along these paths must run at the same time (as its name might suggest), but simply that they must all be completed, before we can continue. The key point is that the order isn’t important to us (if it was, we could use a simple sequence), but that all the elements must be executed.

The AND gateway has a simple interpretation and behavior, so often, there’s no need to show its name on the diagram (although this is allowed). The principle is simple: when a token enters the parallel gateway, it’s multiplied so that each of the sequence flows coming out of the gateway gets its token (without checking the conditions). Since the token is generated independently of the conditions, it makes no sense to add conditions on the paths coming out of this gateway.

However, what happens next with the tokens that are created in this way? Each of them flows further through the process and activates further tasks. This sounds harmless, but one of the biggest pitfalls lurks here for people new to BPMN. Let’s look at Figure 8.17 and consider what would happen in such a process.

![](img/a4c90ee3d142c33743706d37c65779c2_237_0.png)

Figure 8.17 – A risky way of modeling – using only the opening (diverging) AND gateway

To make it easier to describe this situation, let’s assume that task B takes 5 minutes, task C takes 10 minutes, task D takes 15 minutes, and tasks E and F take 1 minute each. There’s no problem with the availability of the resources responsible for these tasks.

The beginning of the process is as we expected: after task A was performed, our token reached the parallel gateway and split into three tokens – one for each path – and thus activated tasks B, C, and D. After 5 minutes, task B was completed (our log is AB for now). Next, the token reaches task E and activates it. After another minute, task E is completed (our log is ABE) and the token reaches task F, which is activated. Another minute passes and the token reaches the end event, where it’s consumed. At this point, our process log is ABEF. Another 3 minutes pass and task C is executed (so we have ABEFC). Its token also goes to task E and activates it, after which it continues until the end event (when the second token disappears; we already have a log of ABEFCEF). Something similar will happen with the third token that goes out of task D. When the last token reaches the end event and thus the process instance (that is, its specific execution) ends, our process log will look as follows: ABEFCEFDEF. As you can see, we have executed tasks E and F three times!

Paradoxically, this situation isn’t a mistake from the point of view of BPMN but the way of modeling, in which we have a splitting AND gateway without a closing (merging) gateway will likely be highly confusing for most people because they won’t expect multiple executions.

