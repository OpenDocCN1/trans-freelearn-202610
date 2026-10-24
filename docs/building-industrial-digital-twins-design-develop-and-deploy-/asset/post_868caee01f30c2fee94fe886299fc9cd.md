

## Agile Manifesto

The Agile Manifesto refers to a document that explains the values and principles of Agile software development. It is visually shown in Figure 3.7 and was first published in 2001. These values and principles are described here: `https://www.visual-paradigm.com/scrum/agile-manifesto-and-agile-principles/`. The Agile methodology provides an alternative to the traditional heavy-weight waterfall method of developing software. The Agile methodology fits the rapid experimentation that Digital Twin initiatives often need. The use of the Agile methodology in the context of Digital Twins, by Siemens and Accenture, is described here: `https://blogs.sw.siemens.com/thought-leadership/2018/01/26/using-agile-processes-and-digital-twin-technology`. We suggest you view the video shared on this Siemens blog page, from the Digital Twin Summit (see `https://youtu.be/ETTlTq88oHU`).

![](img/f445915fceb4649a77b6797f1425850b_96_0.png)

Figure 3.7 – Agile Manifesto

Figure 3.7 summarizes the spirit of the Agile methodology where smaller iterations lead to bigger outcomes while reducing the risks. The development work is often divided into the following:

- Releases
- Epics
- Stories
- Sprints
- Daily standups
- Test cases
- Demo and user acceptances
- Iterations (pivot or persevere)

Another way to look at the Agile methodology at work is visually shown in Figure 3.8, in their natural order:

![](img/f445915fceb4649a77b6797f1425850b_97_0.png)

Figure 3.8 – Scrum methodology

The product backlog is defined in terms of releases, epics, and stories. The stories and epics are worked on during the Sprints, which can often be as short as 1-2 weeks. The Sprints involve their own planning so that stories are mapped to the Sprint Backlog. Once the developer completes the assigned tasks measured in story points, their work is reviewed or may be demoed to the business users. This leads to the wrapup of the Sprint and iteratively working on the product backlog.

As we further explain this terminology, let's use a realistic example. **Western Digital Corporation (WDC)** described their 12-week-long Digital Twin experimentation here: `https://blog.westerndigital.com/digital-twins-optimize-robot-manufacturing-ops/`. They called it a rapid learning cycle with the goal to create an **Autonomous Robot Vehicle (ARV)**. Let's attempt to create the high-level Scrum artifacts for this initiative.

## Release

A release is defined as an application that can be distributed to business users or a control group. This can be beta software or a pilot product or can be a generally available version of the software product. In the WDC case, the release would consist of some version of the ARV application, developed with the help of the Digital Twin process. A release is usually defined in terms of features that are on the product roadmap. These features may be described as epics.

## Epic

An epic is a unit of work for development that logically groups related requests or needs of the user community of the application. Epics help the team to estimate the time and effort required for a logical grouping of work. Epics should be written in a manner that the business users can easily understand and validate at the end of the cycle. In the case of WDC, the epic can be stated as "the ARV pilot using the Digital Twin." Epics are broken down into stories, which can then be estimated more precisely and allocated to the members of the development team.

## Story

The story, also referred to as the user story, is a description of the characteristics of the software application from the different stakeholders' perspectives. Stories are meant to be small units of work but tangible in nature. These stakeholders may be the end users or IT folks who maintain the final solution or the business sponsor of the project. A story should have these characteristics:

- Articulate how the characteristics of the application are valuable to the stakeholders.
- Describe how these can be demonstrated, tested, and verified for acceptance.
- Focus on the "what" and not particularly on the "how." This allows the technical team to figure out the best way to build the application.
- Be easy to estimate the work effort required by the Scrum team, while reducing the risk.

The best practice is to vertically slice the story. To take a simple analogy, a pizza consists of layers of dough, pizza sauce, cheese, and toppings. If a story only contains the dough, the user or the tester will only get to taste the dough by itself and will not be able to provide good feedback on the "acceptance" of the final pizza. On the other hand, by the principle of "vertical slice," if the story is to make a small slice of pizza, then the user can provide a lot more meaningful feedback and the risk to the acceptance of the final pizza will be much lower. While this "vertically sliced story" is highly desired, it may not always be feasible.

To look at the possible stories for the WDC epic, here are some possible stories:

- Twin of ARV movement in the second floor of the semiconductor fabrication plant (fab) of Shanghai, China
- History of the wait time and service time distribution for the **Device Under Test (DUT)** at the fab

This can be the starting point with two stories for the WDC epic. The stories can be finished in one or more Sprints. However, the goal is to be able to demo the progress at the end of each Sprint to the stakeholders.

## Sprints

A Sprint is a short interval of time, often 1-2 weeks. Sprints are used to complete work such as the user stories. A release is often broken down into multiple Sprints. In the WDC example, for instance, the 12-week exercise could be broken down into 6 sprints of 2 weeks each. As these sprints progress, the measurable units of work are completed and tested according to the often-documented test cases.

## Daily standups

A daily stand-up meeting is an Agile ritual used for tracking daily progress. It can be held once or twice a day. When twice, it is at the beginning and then end of the working day. Good communication is key to the success of Agile methodology and the daily standups are the key part of synchronizing the whole team. Often, each member may discuss these three items:

- What work did you do yesterday or today (when the standup is at the end of the day)?
- What work will you do today or tomorrow?
- Are there any blockers to your work?

When the whole or the majority of the team is working from a single office, the team gathers around the information radiator or the Scrum board during the daily standups. Figure 3.9 shows such an information radiator:

![](img/f445915fceb4649a77b6797f1425850b_100_0.png)

Figure 3.9 – Scrum information radiator

> **Note**
> Image source: `http://agileconsulting.blogspot.com/2013/09/agile-transformation-cadence-model_495.html.`

The use of the information radiator during the daily standups and in the area where the Scrum teams work provides a high level of visibility to the overall progress of the Sprints and the releases. In scenarios where the majority of the team is not working under a single roof, electronic versions of such information radiators and Scrum boards are used and the daily standup may take place via video conferencing systems. Figure 3.10 shows one such electronic Scrum board:

## Digital Twin Sprint Burndown

![](img/f445915fceb4649a77b6797f1425850b_101_0.png)

Figure 3.10 – Electronic Scrum board for the Agile methodology

> **Note**
> Image source: http://phdesign.com.au/general/excel-templates-for-scrum-product-and-sprint-backlogs/

While the daily standup has been described in a generic sense, here it will apply to the WDC scenario as well.

## Test cases

Test cases are the scenarios that are used to validate that Sprints are producing work on the stories and epics in a manner that will meet the stakeholder requirements. These test cases can be used by the **Quality Assurance (QA)** team of the users to check and accept the work along the way. The set of conditions or variables used to determine whether a system under test satisfies requirements is created manually from specifications, which are later used to create tests used by the QA team. In the WDC scenario, the test cases will help the stakeholders to validate the progress toward the ARV pilot using the Digital Twin over the Sprints.

