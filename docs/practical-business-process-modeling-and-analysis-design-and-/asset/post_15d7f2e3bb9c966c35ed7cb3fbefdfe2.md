

## BPMN Tools tested for Model Interchange

| Vendor | Tool | BPMN 2.0 | Import | Export | Roundtrip | Demo Participation | Last Test Submission | A Results Submitted | B Results Submitted | C Results Submitted | Schema-valid |
|-----------|---------|-------|--------|--------|-----------|-----------------------|------------|-----------|-----------|-----------|---------|
| Camunda | bpmn.io (Camunda Modeler 16.6.1) | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020, 2018, 2016, 2017, 2016, 2015, 2014, 2013 | ○2025 | ○24/24 | ○18 | ○24/24 | ○14/14 |
| BOC Group | ADONIS 17.0 | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020, 2018, 2016, 2017, 2016, 2015, 2014, 2013 | ○2025 | ○24/24 | ○18 | ○24/24 | ○14/14 |
| Trisotech | Trisotech Modeler Modeler 12.6.3 | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020, 2018, 2016, 2017, 2016, 2015, 2014, 2013 | ○2025 | ○24/24 | ○18 | ○24/24 | ○14/14 |
| OMNINET | OMNITRACKER BPMN 12.3 | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020 | ○2025 | ○24/24 | ○18 | ○24/24 | ○12/14 |
| Visio Unternehmensberatung AG | BPMN-Modeller for Confluence Enterprise 2.36.0 | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020 | ○2024 | ○24/24 | ○18 | ○24/24 | ○14/14 |
| Software AG | ARIS 10.0.22 | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020, 2018, 2018 | ○2023 | ○24/24 | ○18 | ○24/24 | ○14/14 |
| SAP Signavio | SAP Signavio Process Manager 17.5.1 | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020, 2018, 2017, 2016, 2015, 2014, 2013 | ○2023 | ○24/24 | ○18 | ○24/24 | ○9/12 |
| Esteco S.p.A. | modeFrontier (modeFrontier 5.3.1) | ● | ○Diff | ○Diff | ○Details | ○2024, 2022, 2020, 2018, 2016, 2017, 2016 | ○2023 | ○16/24 | ○18 | ○24/24 | ○10/10 |

Figure 7.3 – An excerpt from a table summarizing the results of tool tests performed by the BPMN MIWG

The purpose of the BPMN is to provide a common language for stakeholders interested in the process. In the specification document, the authors state that their goal was to create a notation that would be a _common language_ for various groups of users: business analysts, developers, and business users, both in terms of designing a process and automating it. They wanted to simplify communication between various process stakeholders: people running the process, implementers, customers and suppliers. To do so, they aimed to create a standard notation based on other already-existing notations and using their tried and tested elements.

This is a rather bold assumption, because on the one hand, it should support business users who are interested in creating easy-to-read diagrams, and on the other, it should enable one of the notation’s goals, which is to allow any diagram to potentially serve as a basis for preparing process automation.

The solution proposed by the specification is the concept of compliance levels (inspired by Bruce Silver’s method of teaching BPMN), which divides the elements of the notation into three groups. The first group consists of the most basic elements for documenting processes at the **descriptive level**. Diagrams using these elements are not much more complicated than flowcharts, but they ensure the transmission of information in a clear and standardized way.

The second group includes elements that go beyond what users of flowcharts may be used to – they allow the creation of more sophisticated models, aimed more often at an audience interested in analyzing processes for automation. These elements belong to the **analytical level** of BPMN.

The last group is generally made up of the most advanced elements, which are intended to automate processes, although nothing prevents you from using them for documentation and analytical purposes.

We will learn much more about elements of the BPMN notation in the following chapters (there, you will see more object types described, as well as information about the categories used in BPMN), but for the purpose of comparing it with other notations, we will start by reviewing the basic elements, which will be used in our example diagram. Please note that BPMN diagrams are usually drawn horizontally, but I am using a vertical representation to make it consistent with previous examples.

| Modeling concepts | Shape and name | Description |
|---|---|---|
| Start of the process diagram | Start event | The first element of a process diagram representing an event that starts a process. Please note that all events are circles, but each type has a distinct look. |
| End of a process diagram | End event | The last element of a process diagram representing that an event is the result of a process. Please note that all events are circles, but each type has a distinct look. |
| Process split | Gateway | Split of a process into multiple outcomes. Please note that in BPMN, several types of gateways are possible with different icons inside. A diamond without an icon inside represents the most common type: exclusive gateway. |
| Process step (simple) | Task | A simple process step representing some work to be done in a process. Please note that both tasks and subprocesses are shown as rectangles, but each type has a distinct look. |
| Process step (complex) | Subprocess | A more complex step of a process, which can have several sub-steps – often shown in a separate diagram. Please note that both tasks and subprocess are shown as rectangles, but each type has a distinct look. |

Table 7.4 – Basic shapes of BPMN

Apart from the basic shapes of BPMN, which represent the most important modeling concepts, there are, of course, more which will be covered in depth in _Chapters 8_ and _9_. In the following table, you can see two more shapes, which you will see in the following figure.

| Non-standard modeling concepts | Shape and name | Description |
|---|---|---|
| Data | Data object ![图片] | Data used in a process (also serving as an input or an output for the process) |
| Storage/IT system | Data store ![图片] | Permanent storage of data in a form such as a cabinet or IT system |

Table 7.5 – More advanced shapes of BPMN (selected)

As you can see, there are many similarities between BPMN and previous notations. Now, let’s take a look at the complaint process modeled in BPMN.

## Figure 7.4 – Example process as a BPMN business process diagram (created in ADONIS)

# Process Modeling Notations

As you can see, BPMN took many of the proven methods introduced by other notations and extended them, so that the following apply:

- Start and end events are both represented as circles but can easily be distinguished from one another based on the border thickness.
- You read a process diagram by following an arrow called a sequence flow drawn with a solid line (we will cover additional types of flows in the following chapters).
- Process steps (also known as activities) are rectangles with rounded edges. Those that do not need to be decomposed are called tasks, and those that are more complex (usually marked with a “+” icon) are called sub-processes.
- Places where a process diagram splits are called gateways, and they follow the common shape of a diamond.
- You can easily see who does what because elements of the BPMN diagram are placed inside pools and lanes representing process participants (e.g., companies) and more detailed logical divisions of those participants (e.g., departments or teams).

Therefore, if you have already modeled a process using other notations, chances are that you will be able to read BPMN diagrams pretty easily.

However, BPMN offers you many more options in comparison with other notations. There are also pitfalls – places where shapes and parts of models may look familiar, but in BPMN, they have a slightly different meaning.

That’s why in the following chapter, we will dive deeper into BPMN and learn how to use it properly – beginning from the basics.

# Summary

In this chapter, you learned about different ways of modeling processes and how to read process diagrams. You saw how BPMN uses proven approaches from other notations and how it differs from those notations so that you can easily grasp visual differences in diagrams.

In the next chapter, we will learn more about the things that you need to know about BPMN to use it in practice.

