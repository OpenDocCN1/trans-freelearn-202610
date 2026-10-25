
Existing process documentation can also be helpful as a source of knowledge for process analysts. However, sometimes it is not up to date, so it needs some additional work to make sure that it is known what is really happening. It is a good starting point, but sometimes it needs to be validated and should not be used as the single source of knowledge about the process.

In the past few years, a new approach to gathering information about as-is processes has gained popularity: **automated process discovery**. By using **process mining** and **task mining** techniques, it is possible to analyze the logs from the IT systems automating the process or analyze the screens of employees performing the process to come up with a data-backed version of what the process really looks like.

Of course, you should be aware that while process mining offers you many exciting possibilities for advancing your process management to the next level, it is not a panacea for process discovery. That’s because for a significant percentage of organizations, there are still many processes with parts that cannot be analyzed in an automated way.

The next approach is to create a process model by involving teams performing the process as well as the **process owner** in a workshop. This requires careful coordination because it is not easy to have several busy people attend the workshop at the same time. However, by involving all interested teams in the modeling, we create a model that considers everyone’s voice. Such a model is a kind of contract between all the teams working together to document their shared understanding of a process. The participation of many people provides the opportunity to clarify any ambiguities and misconceptions. In addition, working together on the model helps build commitment among the participants.

Interestingly, some organizations also hold workshops online with copilots providing input for creating process documentation based on transcripts.

It is also worth mentioning that sometimes it makes sense to speed up the modeling by using existing reference models or best practices. This is especially helpful when we are not trying to document the existing process but want to design a new one and don’t want to do it from scratch.

## Processes of process modeling

Since we now know how a process model can be created, it would also make sense to briefly discuss what will happen when we already have the model. An organization that wants to manage its processes can benefit from defining some processes it will use in process management. While there are many such processes, I will mention the most common ones.

First, we need a process that will help us identify what processes we have in the organization. Once we have done this, we should plan which processes require the detailed modeling that was described previously. This may be needed because we do not have a model at all or because the process model needs to be changed, for example, due to a process improvement initiative.

When the process model is ready, usually there will be some formal steps needed for approving it by the process owner, but possibly, other roles can be involved too. The approved model should be easily available to **process performers**, so that they can use it in their daily work, but also so that they can send feedback and improvement ideas for the process.

More advanced organizations often have processes for monitoring the **key performance indicators** and/or periodic reviews of the process as well. This can trigger a need to improve a process, which will also require an update or a complete redesign of the process model.

As you can see, there are many things needed to make sure that process models are up to date and useful for the organization. To make sure that all this work is not a burden for the organization, proper tools should be used that will support working with the process documentation.

## Tools for process modeling

Since we now know who can do the process modeling and how this knowledge about processes will be gathered, it’s time to discuss how the process models can be created and what kinds of applications can be used.

First of all, it is worth noting that it is not obligatory to use IT tools for process modeling. There are some organizations (especially those with a strong Lean or Agile culture) that prefer to use tools such as flipcharts, whiteboards, and sticky notes.

However, in my experience, while those tools are great for workshops and making people engaged in process improvement initiatives and visual management, it is rather difficult to avoid using software if process management initiatives are broader and involve more people from different locations. That’s why we will focus on software supporting process modeling.

There are many IT tools that can be used to create process models. Currently, more and more tools are available on the cloud via a browser, but there are still many standalone applications that you can download on your computer.

Organizations will have different modeling scenarios and different visions of how processes should be documented, how this documentation should be published, and how to make sure that it will be a living system capable of gathering feedback and ideas, as well as supporting changes. That’s why there’s no one tool that would work best in every case and help get the whole organization involved in BPM. So, instead of giving you a recommendation for one single BPM tool to rule them all, I will provide an overview of various tool categories with information about their advantages, so that you can decide what would be best for you.

Some organizations start their adventure in process management by creating process documentation with general-purpose office suites. This mix of text documents, shapes representing processes, and so on is very easy to create, but keeping it up to date is very cumbersome and time-consuming. Also, it is very difficult to keep people engaged if the process documentation is available as slides, text documents, and spreadsheets.

That’s why usually, more specialized tools are used. In my experience, there are four broad categories of applications that can be used by organizations that want to document their processes:

- Diagramming tools
- **Enterprise business process analysis (EBPA) tools**
- **Model-based software engineering (MBSE) tools**
- Modeling tools linked with process automation suites

Let’s look at each of these in greater detail.

## Diagramming tools

As you could expect, **diagramming tools** allow for much easier creation of diagrams than office suites. Usually, they are very user-friendly tool suites with dozens of diagram types and hundreds of different templates that allow you to create a diagram for nearly any purpose you can dream of, from strategy planning to planning the layout of the office.

Of course, they also allow you to create many kinds of diagrams that can be useful for process management: high-level process landscapes, detailed process maps (using various notations), organizational structure diagrams, and so on.

Basic tools of this kind focus on creating nice-looking pictures with smooth collaboration, but they usually offer very limited features in terms of process analysis and governance (e.g., approvals).

More advanced diagramming tools can also be integrated with external systems, for example, to reach a broader audience with process documentation or to import live data and visualize it in the diagrams. This makes them closer to the next category of tools: EBPA.

Example tools of this kind that I frequently encounter are the following:

- **Draw.io/Diagrams.net (JGraph Ltd and draw.io AG)**
- **Lucidchart (Lucid Software)**
- **Visio (Microsoft)**

## EBPA tools

EBPA tools are much more specialized than diagramming tools. They allow you to create various types of process models very easily, but they are much more focused, so you can expect many types of models that you may need to create a comprehensive process documentation, but not as much in diagramming tools.

Those models are usually much richer in attributes in comparison with diagramming tools. While a simple diagramming tool creates a diagram or a process map that is a nice-looking picture, a process model usually has strictly defined rules of what is allowed and what is not. Also, model types available in EBPA tools are much more advanced and contain special attributes, and features aimed at different roles involved in process management, and so on.

Examples of EBPA tools are the following:

- **ADONIS** (BOC Group)
- **ARIS** (Software AG)
- **iGrafx** (iGrafx)

ADONIS is the tool I used to create most of the examples in this part of the book. This is the tool I know the best, since I work at BOC Group. If you wish, you can use the free version of the tool, called ADONIS Community Edition. The registration page can be found at `https://www.adonis-community.com`.

With Visio, it is worth noting that it has some features that exceed the standard capabilities of drawing tools. For example, Visio BPMN diagrams can be exported to Power Automate.

It is also worth mentioning that simple process documentation could also be created with **collaboration boards** such as Miro, Mural, or Klaxoon. Both diagramming tools and collaboration boards excel at visual collaboration over diagrams.
