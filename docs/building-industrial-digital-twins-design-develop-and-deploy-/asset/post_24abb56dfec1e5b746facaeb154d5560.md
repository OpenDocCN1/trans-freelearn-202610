

The digital value to society focuses on three impact points:

- The traditional economic measures for customers and employees in terms of cost, time saving, and efficiency improvements.
- The societal impact in terms of job creation, new skills development, reduced traffic congestion, and safer working environments.
- The environmental impact can be described by reducing CO₂ emissions or improving the management of tailings in mining:

![](img/f445915fceb4649a77b6797f1425850b_47_0.png)

Figure 1.12 – Example of a value at stake analysis for a Digital Twin

> Note
> The preceding diagram has been adapted from https://reports.weforum.org/digital-transformation/introducing-value-at-stake-a-new-analytical-tool-for-understanding-digitalization/.

The single-page view of the value at stake for a Digital Twin, shown in the preceding diagram, provides a simple yet powerful way to describe a Digital Twin’s value proposition.

Now that we can describe the value of a Digital Twin, the next key step is identifying ideal candidates for Digital Twins in your business.

## Identifying opportunities

Digital Twins focus on addressing specific business challenges or exploiting new opportunities, as we saw in the value at stake description of Digital Twin applications. These challenges and opportunities provide guidance for identifying opportunities for Digital Twins in industrial applications. This will be covered in more detail in _Chapter 4, Getting Started with Our First Digital Twin_.

The remainder of this book will focus on selecting and building your first Digital Twin, but let’s provide a summary of the high-level guiding principles that can be used to identify a potential pilot from a pool of candidates.

For Digital Twins that focus on improving asset performance, reducing downtime, and increasing production or throughput, the ideal starting point is to identify current "bad actors." This approach is based on using current failure data, production loss information, or failure mode analysis on previous downtime incidents. Applying the 80/20 Pareto principle will help you identify the initial shortlist of entities that cause the bulk of the downtime.

> **Important Note**
> The Pareto principle states that for many outcomes, roughly 80% of consequences come from 20% of the causes (the "vital few"). Other names for this principle are the 80/20 rule, the law of the vital few, and the principle of factor sparsity: `https://bit.ly/DTPareto8020`.

Digital Twins that focus on exploiting new revenue opportunities are generally more strategic and have well-described business cases. The technical feasibility of these opportunities is typically also a factor when designing the new service. Real-time data access, sensor information, and other functions of the Digital Twin are built from the start.

The next step for both scenarios is to rank the business impact against the Digital Twin’s technical feasibility. Technical feasibility is generally a factor of infrastructure, connectivity, data access, appetite for change, and organizational maturity.

It is a high-level ranking that can easily be done in Excel, and a template is available at https://bit.ly/DTPriority:

## XMPRO Business Value Assessment

| # | Use Case/Scenario | Safety | Downtime | Throughput | Quality | Cost | Economic Value/year | Automation | IT Systems | Analytics | Environment | Project |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Use Case 1 | Medium | High | High | High | High | > $10m | High | High | Medium | High | High |
| 2 | Use Case 2 | Low | Low | Medium | customer satisf | High | > $10m | High | High | Low | High | High |
| 3 | Use Case 3 | Low | Low | Low | Low | Low | > $1m | High | High | Low | High | High |
| 4 | Use Case 4 | Low | Low | Low | Low | Low | > $1m | High | High | Low | High | High |
| 5 | Use Case 5 | Low | Medium | High | Low | Medium | > $10m | Medium | High | Low | High | High |
| 6 | Use Case 6 | Medium | Medium | Medium | Medium | Medium | > $1m | High | High | High | High | High |
| 7 | Use Case 7 | Low | Medium | Medium | Medium | Low | > $1m | High | High | High | High | High |
| 8 | Use Case 8 | Medium | Medium | Medium | Medium | Medium | > $10m | High | High | High | High | High |
| 9 | Use Case 9 | Medium | High | High | High | High | > $1m | High | High | High | High | High |
| 10 | Use Case 10 | High | Medium | Low | High | Low | > $1m | Medium | Medium | Medium | High | High |

Figure 1.13 – Business impact and technical feasibility assessment

In this example, the following technical assessment criteria are being used:

- OT complexity
- IT complexity
- Analytics
- System complexity
- Project readiness

The technical assessment criteria can be adjusted to fit the business’s requirements, but for this example, the criteria are for a typical industrial installation:

## Digital Twin Prioritization

![](img/f445915fceb4649a77b6797f1425850b_49_0.png)

Figure 1.14 – Digital Twin prioritization matrix

## Summary

In this chapter, we started by taking a brief look at the history of Digital Twins and the original intent behind the concept. We discussed the difference between discrete and composite Digital Twins, and then we identified the elements required for something to qualify as a Digital Twin. We introduced several typical use cases across the life cycle of a Digital Twin, discussed the difference between a Digital Twin and a Digital Thread, and provided guidance on describing the value proposition of a Digital Twin. Our Digital Twin prioritization matrix provides a framework to perform a high-level assessment of the use cases for your first DTP.

You now have an understanding of what a Digital Twin is, its key characteristics, the value of a Digital Twin, and assessing where a Digital Twin may apply in your business.

The remaining chapters will guide you through building your first Digital Twin, beginning with planning your Digital Twin, identifying the right candidate for your first Digital Twin, and then providing guidance on setting up, building, deploying, and validating the outcomes of your DTP.

This order of magnitude is visually represented in a bubble chart, with the business impact and technical readiness scores as the two significant measures. The weighted average values of each of these measures are placed on the graph, which is divided into four quadrants. The value of the economic impact determines the size of the bubble. The four quadrants represent the business readiness for each of the Digital Twin scenarios. The **Do Minimum Viable Product (MVP)** quadrant represents a high business impact and a high level of technical readiness.

The opportunities on the quadrant’s far right-hand side, with the biggest bubble size, often represent Digital Twin projects with the highest likelihood of success for all stakeholders.

This is a very simple approach to ranking possible candidates for your first Digital Twin; the next chapter will provide more guidance on planning your Digital Twin project.

## 2 Planning Your Digital Twin

In the previous chapter, we learned why we need Digital Twins and how to use them to drive specific business outcomes. We explored the history of the Digital Twin and the various industries in which it presents opportunities.

Now, let’s examine how to plan for an industrial Digital Twin in an enterprise setting. We will identify the key criteria that can be used to determine whether the industrial Digital Twin is applicable to the business scenarios. Additionally, we will explore how to develop the business case for investments in the Digital Twin. Following this, we will explore the prerequisites for the Digital Twin in an enterprise, including the functional and non-functional requirements. This will allow us to identify the underlying digital technologies required for the Digital Twin. Note that we will not lose sight of the organizational factors that are in play for the success of the industrial Digital Twin initiatives.

In this chapter, we will learn about the following topics:

- Key criteria
- Expected business outcomes
- Prerequisites for the Digital Twin
- Organizational factors
- Technological needs

