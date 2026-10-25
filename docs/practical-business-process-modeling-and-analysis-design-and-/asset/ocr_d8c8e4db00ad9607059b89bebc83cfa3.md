There are many different types of events in BPMN, some of which are useful for more advanced modeling scenarios (for example, process specification/automation), and some of which are used occasionally in practice. Therefore, in this chapter, we’ll only focus on a few of the most common types of events, without which it’s impossible to model in BPMN. This will help us understand the basics of how events work.

# Simple start event

Start events are always shown as circles with a single thin line. The following is a start event with no trigger defined.

![](bbox=[0.441, 0.297, 0.512, 0.354])

Figure 8.30 – Start event (no trigger)

Let’s start with a simple start event. The start event is always represented as a circle with a single thin border. It represents what must happen for the process to start (technically speaking – what needs to happen so that a process instance or execution of a specific “case” can begin). In this case, the start event doesn’t have a specific trigger defined (which is easily visible because there’s no icon inside).

All start events are catching, which means that they passively listen to what’s going on, and only when something happens that triggers them will they create a token that flows further and makes the process begin. Before the start event, there’s nothing (at least from the perspective of the diagram of the process), so there can be no sequence flow arrows incoming to the start event.

# Naming start events

The BPMN specification doesn’t impose a specific way of naming start events. I like to name events so that their name clearly describes a certain state of reality that must exist for the process to start.

Various other approaches are also encountered in practice (shown in Figure 8.31), some of which may, for example, be inspired by flowcharts (START). While none of the examples shown are wrong from a BPMN point of view, they differ in their informational value. I like to use the passive side, which avoids confusion with the work that’s done in the process. So, I could give the start event without a type a name such as Summary of reasons of complaints needed (for a process related to our complaint handling process) or New employee needed (for a process in which new employees are recruited).