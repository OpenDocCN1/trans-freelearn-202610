# # Simple end event

End events are also shown as circles. However, they have a border drawn with a single thick line. The following is an end event without any trigger.

![](bbox=[0.4434, 0.1797, 0.5156, 0.2383])

End event

Figure 8.32 – End event (no trigger)

Now that we know how to use start events to show what starts our process, it’s a good time to look at the other end of the spectrum and think about how our process might end. End events are the second element of the process scope. They illustrate what result our process might end with. Start events are, by definition, passive elements. End events are their opposite – they’re always active, which in the BPMN specification is referred to as throwing.

As we know, there can’t be anything before the start event in a process flow, so a sequence flow arrow can’t enter it; only outgoing flows are allowed. On the other hand, in the case of an end event, we can’t place any flow objects after them, such as events, gateways, or activities. Therefore, a sequence flow arrow can enter the end event, but there can be no outgoing sequence flow from it.

Each end event will have a border drawn with a distinctive single thick line, which allows us to easily distinguish it from the start event (which always has a single thin line).

# # Naming end events

As in the case of start events, the BPMN specification doesn’t impose special ways of naming end events, so we have quite a bit of freedom. My preference is to name end events so that it’s clear what result the process ends up with – that is, what we achieved. Ideally, this name should emphasize what _happened_ in this process path and not something we _do_. This way, we can see _how_ the process ended, not just that it’s finished, as shown here.