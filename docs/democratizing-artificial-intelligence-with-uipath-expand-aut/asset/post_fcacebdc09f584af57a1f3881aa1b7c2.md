
The following diagram shows the available technologies across the spectrum of office work:

![](img/05268897c8ac5d80eb3fead1cc8a822a_40_0.png)

Figure 2.2 – The automation technology stack

RPA, application programming interface (API), ERP, and business process management (BPM) belong to a class of automation technologies that reduce the manual efforts of rule-based digital activities. AI, ML, and NLP belong to another class of automation technologies. They reduce the manual effort of activities that require cognitive thinking.

As you learned in the preceding section, almost all activities have rule-based and cognitive components. Until recently, there were no platforms that allowed for seamless integration of these different technologies. Businesses look for a technology platform that allows users to take advantage of the different automation technologies. A new class of automation products called the cognitive automation platform has emerged. The cognitive automation platform simplifies the integration between systems and technologies. It also makes AI easy to develop and consume and bridges the gap between RPA and AI.

## Practical tips

There are tremendous opportunities to apply appropriate cognitive automation to the spectrum of work to generate benefits. The most common mistakes and solutions of early cognitive automation adopters were these:

- They did not look at business problems holistically and were constrained by artificial boundaries such as **business units (BUs)**. This hurdle required upper-level engagement to break through business silos.
- They wanted to test and deploy a specific technology and retrofit business problems to the technology. Many companies now have the mindset of business problems first and technology second.
- They were too optimistic about AI technology maturity and overestimated the AI capability. A better understanding of the different technologies and how they work would help set the appropriate expectation.
- They underestimated the importance of human elements, replacing delicate human touchpoints with technology without giving thought to the impact of such a replacement. Most recent digital transformation journeys include human-centric design in their discipline to avoid this problem.

By now, you should have a good grasp of the spectrum of work, the automation potentials, and the technology stack. Let's reimagine cognitive automation by perfecting human-machine collaboration.

## Designing human-machine collaboration with cognitive automation

In the previous section, we explored the spectrum of office work and its relative automation potentials. Companies look for a more efficient and effective way to perform office work by leveraging automation. Office work does not comprise a single *activity* but a sequence of activities that make up a *process*. Full automation of the **end-to-end (E2E)** process may be the eventual goal. However, a more realistic goal is good *human-machine collaboration* to drive efficiency and effectiveness with currently available technology. Therefore, it is important to understand how humans and machines work differently and the best way to think through a human-machine collaboration when designing cognitive automation.

- 3. *Enter data into the contract system (data processing)*: This step is automated. RPA completes the steps to log in to the system and input data elements extracted from the contract according to a set of rules.
- 4. *Find non-compliance contract terms and resolve (applying expertise and stakeholder interactions)*: This step is partly automated. The document understanding ML model has NLP to extract relevant contract terms from contract documents. However, humans are responsible for the interpretation and discussions with stakeholders to resolve non-compliance issues. Machines and humans work together to complete this step.

The second example of **loan processing** shows four steps that require different skill sets in the spectrum of work, as follows:

- 1. *Receive loan applications via a customer portal (data collection)*: This step is automated. An API routes data from the customer portal to the ML model.
- 2. *Run ML model to determine preliminary approval statuses (applying expertise)*: This step is automated. The ML model determines approval status based on past historical data. The ML is fine-tuned over time with new training data to ensure prediction performance. RPA routes preliminary approval statuses to either the exception case queue or the processing queue.
- 3. *Review exception cases (applying expertise and stakeholder interactions)*: This step is partly automated and is an example of a human in the loop with assisted RPA. Loan officers review cases that have lower confidence from the ML model. RPA pulls additional data for loan officers as reference material, along with the exception cases. Loan officers leverage their experience and the additional data to make final approval decisions. Machines and humans work together to complete this step.
- 4. *Process approved and rejected cases (data processing and stakeholder interactions)*: This step is partly automated. RPA processes the loan applications according to the approved and rejected workflows. In some cases, loan officers contact the customers to review the decision and make recommendations. Machines and humans work together to complete this step.

## Applying differences between how humans and machines work in cognitive automation design

Humans and machines behave and perform differently. The following figure illustrates how humans and machines function in problem solving, memory, productivity, and emotions. These differences influence how we design cognitive automation to achieve a seamless human-machine collaboration:

| | Human | Machine |
|---|---|---|
| Problem Solving | Heuristics: Depends on a rule of thumb, or a mental shortcut | Algorithm: Depends on a series of sets of defined steps |
| Memory | Short-term / Working Memory of ~7 pieces of information | Depends on RAM (Random Access Memory) capacity |
| Productivity | Duration of uninterrupted time | Any available runtime |
| Emotion | Ability to feel and empathize | No feelings and empathy |

Figure 2.4 – Humans versus machines

Let's take a deeper dive into each of these areas, as follows:

- **Problem solving**

*Cognitive automation design implication:* The first step is to assess the type of problem at hand to determine the best-suited solution. If the problems are predictable and have historical learnings, current automation technologies are more likely to be effective, whereas if the problems have many unexpected options, humans are more likely to be effective. With either humans or machines, the key to perfecting the algorithm is trial and error.

In the following figure, you can see more information on how humans and machines solve problems:

## Problem Solving

![](img/05268897c8ac5d80eb3fead1cc8a822a_45_0.png)

![](img/05268897c8ac5d80eb3fead1cc8a822a_45_1.png)

- Humans solve problems using **heuristics**. Humans rely on the rule of thumb and the mental shortcuts to solve problems. This approach allows humans to solve problems efficiently without exhausting millions of possibilities. In addition, heuristics allow humans to come to an acceptable solution for unexpected situations.

- Machines solve problems using **algorithms** with output that is predictable and reproducible. Algorithms are a series of finite defined steps. Algorithms are best for predictable inputs and outputs.

Figure 2.5 – Problem solving: humans versus machines

## Memory

*Cognitive automation design implication:* Humans process records one by one because of the limitation of short-term working memory. Machines have the choice of processing records one at a time or in batches. Using the same example, the design for machines is different than for humans.

- For humans, the pattern could go like this:

- Row 1: Step 1, Step 2
- Row 2: Step 1, Step 2
- ...
- Row 100, Step 1, Step 2

- For machines, the pattern might look like this:

- Step 1: Row 1, Row 2, ..., Row 100
- Step 2: Row 1, Row 2, ..., Row 100
