
The operating model elements of Zero Trust effectively provide the glue that holds Zero Trust together. These include the following:

- **Business operating models** control organizational focus and rhythms. These models determine funding, metrics, and support from senior leaders and mid-level management. Zero Trust initiatives don’t often change the operating model but must discover it, understand it, and align it with how the organization actually works.

You don’t want to allow autonomy if the organization is one single entity (as that will fracture the strategy and execution). You also don’t want to waste effort trying to impose control across autonomous BUs (which can and will choose to ignore you when they want, regardless of how hard you try).

- **Governance** lays out the guardrails to keep people on track, enabling them to work together seamlessly. Governance determines decision rights (who makes which decisions) and lays out process frameworks to connect teams and guide how they work and interact with each other. The governance process frameworks operate at strategic, team, and practitioner levels in the organization.

*Figure 8.12 shows how these governance process frameworks tie together:*

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_169_0.png)

**Figure 8.12 – Different parts of a Zero Trust governance process framework**

- **Cultural norms** create a CL organization by establishing learning processes and instilling values into the organizational culture. You must ensure that your organization’s cultural framework establishes and reinforces a shared value of CL. A CL culture is critical to agility as it enables people to acquire the right skill sets as they go while executing efficiently.

*Without a culture that reinforces the importance of a value, policies and rules will always fall by the wayside and fail.*

Because of the disruptive nature of Zero Trust and digital transformation, we must ensure people have clarity on this new (and potentially unfamiliar) cultural norm. Reinforce the message and signal its importance by delivering and modeling it consistently on all channels—in processes, metrics, the wording you choose in team meetings, and so on.

The following diagram describes the points in the business operating model where you integrate measures such as governance to sustain Zero Trust:

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_170_0.png)

Figure 8.13 – Zero Trust operating model process

Strategy influences the whole organization, the portfolio applies to all initiatives, and then each initiative includes a design/development, deployment, and runtime (ongoing operations) phase.

Let's look at an Acme example to illustrate how to apply these Zero Trust operating model elements in the real world.

## Acme example – operating model pillar

How does Acme Bank weave in Zero Trust and become a CL organization across the whole enterprise? Figure 8.14 illustrates the operating model pillar at Acme Bank:

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_170_1.png)

Figure 8.14 – Acme Bank's operating model pillar

First, the leadership assessed the current **business operating model** and determined that they currently have a *unified model* enterprise with a single unified enterprise approach across all business lines in the organization.

The unified model means that all stakeholders (including the IT and business organizations) must undertake a common journey and work together as a single organization. This also influences all technology decisions (such as component reuse, infrastructure purchases, and so on).

A key enabler and success criterion for an enterprise Zero Trust initiative is the development of a framework for **governance and guardrails**. (Note that some organizational cultures favor the use of the term *guardrails* instead of *governance* to signal a proactive empowerment approach to oversight.) Governance and guardrails permeate people, processes, and technology—laying the foundation for people to work together, for risk to be managed, and for changes to be well understood.

*Acme* started its Zero Trust governance framework by capturing **decision rights and accountabilities**—which is effectively a modern agile approach to what was once handled by a **Responsible/Accountable/Consulted/Informed (RACI)** framework. *Acme* started with existing business decision makers in control of business assets, functions, and business processes, then added Zero Trust-specific decision owners.

The governance framework also includes the organizational structure for Zero Trust and guardrails for the following:

- *Strategic process model* covering strategic processes (such as developing and maintaining the roadmap)
- *Operational processes* for things that are in use (including IT operations and security operations)
- *Development processes* to ensure the reuse of Zero Trust components

Concurrently, *Acme* is moving toward establishing a CL organization through **training and cultural change**. *Acme* decided to build on its existing training infrastructure but pivoted to preferring **Massive Open Online Courses (MOOCs)** and favoring **just-in-time (JIT)** training over prescheduled courses when possible. *Acme* also focused on gamification and rewarding continuing professional education rather than static qualification and certification approaches. *Acme* also defined, funded, and staffed an internal **communication plan** that detailed communication vehicles (webinars, podcasts, and so on) and ensured Zero Trust communications are done on a recurrent, consistent basis.

*Acme's* leaders also hold themselves accountable for CL using those same courses, and *Acme Bank* executives are encouraged to publicly discuss their own mistakes and learnings during this transformation to give "social permission" for everyone in the organization to do the same.

Finally, Acme is extending the organizational core values to include a **Zero Trust value system** that integrates concepts such as *least privilege*, *assume compromise*, and *verify explicitly* into the organizational thinking, language, and metrics, such as performance reviews.

In summary, the operating model keeps Acme's Zero Trust strategy and execution (operations) on track. This is done by integrating Zero Trust into the business operating model and governance mechanisms and by reinforcing a culture of CL.

## Stitching it all together with the Zero Trust Playbook

Now that we have all three pillars of the model, we can put them together and move forward on making it real at scale. We now implement *sustainable and effective* Zero Trust capabilities that all fit together in an actionable, living, breathing strategy—the Zero Trust Playbook.

We will next look at the overall system of three interconnected pillars and their individual components. We will introduce the six-stage plan in Chapter 9, The Zero Trust Six-Stage Plan, and then proceed to build it throughout the rest of the playbook.

Figure 8.15 describes each playbook component and how they all flow together, much like a blueprint describes all aspects of a building's architecture:

![](img/c2bdb6ffe8b251139bc9db4439c8ed5a_172_0.png)

Figure 8.15 – Zero Trust Playbook overall components and relationships

This diagram explains the relationships in the overall system:

- Each of the dark green boxes at the top of the pillars represents an existing business element of the organization, the *business strategy*, *business operations*, and *business operating model*.
- In the **strategic** pillar, *business capabilities* and *technical capabilities* align with and support the *business strategy*. The *Zero Trust capabilities* you define (starting with The Open Group Zero Trust Reference Model) provide modern digital era security to *business capabilities* and *technical capabilities*, linking them together. Note that existing technical security capabilities are included in *technical capabilities*.
- In the **operational** pillar, *business operations* are informed by the *business strategy* and *business operating model* from the other pillars:
  - In the left column of this pillar, the *business constraints* you operate under are informed by *business capabilities* (in the strategic pillar) and are aligned to *business operations*. *Technical constraints* are informed by *technical capabilities* (in the strategic pillar). The *Zero Trust solution architecture* is built based on *Zero Trust capabilities*, *business constraints*, and *technical constraints*.
  - In the right column of this pillar, *business operations* come to life as *business processes* (and *business functions*), while the *technology estate* reflects *technology constraints*. *Zero Trust services and components* are hosted in the *technology estate* and interact with *business processes*.
  - The *Zero Trust integration* drives cross-cutting changes in *business processes*, the *technical estate*, and *Zero Trust services and components*.
- In the **operating model** pillar, *governance* and *guardrails* keep Zero Trust on track, critical components of which are *decision rights and responsibilities*, which ensure clear ownership of decisions. *Training and culture change* helps support *governance* and *guardrails* and drives the transformation to a learning organization.

## Zero Trust integration drives changes

*Zero Trust integrations* are how *Zero Trust services and components* realize value from Zero Trust investments. This integration drives cross-cutting efforts that ensure Zero Trust services and components become a normal part of business processes, business functions, and the technology estate of the organization. “Cross-cutting” concerns are those that span several of these elements (processes, functions, and technical services) within the organization:

- **Adaptive access example**: Take an example such as modern policy-based adaptive access control that explicitly validates the trust of the user account and the device
