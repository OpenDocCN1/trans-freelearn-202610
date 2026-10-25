# # Script task

Another type of task in BPMN is the **script task**. Tasks of this type have an icon of a written scroll of paper, or simply a script.

![](bbox=[0.4023, 0.1777, 0.5938, 0.2539])

Figure 9.35 – Script task

Like a service, it is a process element that is executed automatically, but in a slightly different way. As intended by the authors of the specification, a service is a step in which the process automation engine calls out external applications that provide certain services. A script, on the other hand, is a step in which certain simple actions are performed directly by the process execution engine.

Examples of such elements include simple validation of user-entered data and running simple calculations. These are only theoretical recommendations; however, as in many models, these principles are not followed. In some tools, service tasks are used to call specific classes of Java code. In general, service tasks are much more common in BPMN diagrams than script tasks.

# # Call activity

The last type of task is the **call activity task**. It is unique in many respects. First, it does not have an icon specific to its type and instead features a thick border, identical to that of a call activity sub-process.

![](bbox=[0.4023, 0.5781, 0.5938, 0.6543])

Figure 9.36 – Call activity task

BPMN allows us to have call activities that are tasks or sub-processes. In both cases, it refers to reusing a certain standard, but in a different way.

The sub-process call activity, which allows us to call other process diagrams, is an extremely useful and popular element of BPMN. On the other hand, task call activity is neither very helpful nor popular.

What is the reason for this? The authors of the BPMN specification wanted to make it possible to show that certain standardized activities occur in processes. Their idea makes it possible to define certain types of tasks (e.g., user, manual, script, and business rule tasks) as so-called **global tasks**, serving as a kind of definition of how things should be done.