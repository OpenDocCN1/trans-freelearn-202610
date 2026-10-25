![](bbox=[0.139, 0.094, 0.866, 0.447])

Figure 9.12 – Conditional events

The two pairs of twins we covered so far (message and signal, as well as timer and conditional) allowed us to discuss the most important uses of events in BPMN. However, there are more trigger types available, as well as some other ways of using events that we have not covered so far. In the remainder of this chapter, we will briefly discuss the more technical and slightly more complicated uses of events. We’ll start with another pair of triggers, but this time, they are not as closely related as the ones we’ve already learned about.

# Escalation events

The ability to describe error and exception handling is extremely important when modeling processes for automation. This allows us to specify what should happen if something undesirable occurs in the process so that we can handle it properly.

Importantly, BPMN distinguishes two types of problems of different severity: less serious escalations and very serious errors (which we will cover shortly). An **escalation** is always indicated by an upward- facing arrowhead icon (which reminds me a bit of the _Star Trek_ insignia).