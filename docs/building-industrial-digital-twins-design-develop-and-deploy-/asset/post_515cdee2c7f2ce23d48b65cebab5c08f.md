

## Section 2: Building the Digital Twin

In part two of the book, we will look at how to go about building the first Digital Twin and then test and validate it, relative to the intended purpose of the twin, as well as how to stabilize the operations of the Digital Twin and eventually scale this process.

This section comprises the following chapters:

- Chapter 3, Identifying the First Digital Twin
- Chapter 4, Getting Started with Our First Digital Twin
- Chapter 5, Setting Up a Digital Twin Prototype
- Chapter 6, Building the Digital Twin Prototype
- Chapter 7, Deployment and Value Tracking

## 3 Identifying the First Digital Twin

In Chapter 2, Planning Your Digital Twin, we discussed the key criteria for Digital Twins in enterprises and their expected business outcomes. We looked at this from the perspective of different industry sectors and their ecosystems. We analyzed the prerequisites for Digital Twins, including the organizational and cultural factors. Finally, we went over the technology requirements for Digital Twins.

In this chapter, we will discuss the evaluation process for a Digital Twin in the context of a wide range of companies. This will provide you with wide exposure to the relevance of Digital Twins both for internal and external opportunities. We will tie this to the actual roles and responsibilities being created and advertised by companies today. We will cover these topics:

- Evaluating Digital Twin candidates
- Roles and responsibilities
- Experimentation and interactions
- The finalist

In a nutshell, this chapter will cover the process of arriving at the first Digital Twin to build, in alignment with the nature of the business and achievable outcomes. This makes the identification and evaluation of the first Digital Twin crucial. This process will be covered comprehensively here.

## Evaluating Digital Twin candidates

In this section, we will identify the prospects for industrial Digital Twins and then evaluate the one we want to move forward with. This evaluation has to be done in the context of your company or initiative, to be relevant to you. We will do the evaluation for multiple scenarios here and that should help for a wide range of cases.

Here are a few settings where a Digital Twin may be evaluated:

- An industrial conglomerate
- A large enterprise in a single industry sector
- A public sector entity
- A large software company or public cloud provider
- An Independent Software Vendor (ISV)
- A large System Integrator (SI) or management consulting firm
- A small/niche service company

Let’s now look at each of these scenarios.

## Industrial conglomerates

Companies that would fit in this category are General Electric (GE), Siemens, ABB, Hitachi, Honeywell, Johnson Controls, Schneider Electric, Bosch, and Emerson Electric. Such industrial global companies have in recent years created a digital competency center within their company, with the goal of accelerating the industrial digital transformation. Some examples are GE Digital and Honeywell Connected Enterprise. We will look at both scenarios where the digital organization is evaluating the Digital Twin candidate or the Line of Business (LOB). Let’s look at the digital organization first.

## Digital twin at digital competency

The digital competency is usually a horizontal LOB with the goal of servicing different LOBs as well as, in some cases, external customers directly, with the goal of developing new markets or new digital revenue streams. When the digital group evaluates the candidates for a Digital Twin, it will look at considerations such as the following:

- Which LOB has identified one or more business use cases for the Digital Twin?
- Is the Digital Twin going to drive internal efficiency in the LOB or is it going to be adopted by its customers?
- Is the LOB willing to commit its Subject Matter Experts (SMEs) to work with the digital group?
- Will the Digital Twin framework be reusable for another LOB?
- What is the current status of the data from the physical asset, such as availability and ownership of the data?
- What is the current status of the physics- or math-based models and do these need to be developed from scratch for the Digital Twin of the physical asset or the process?

Now that we have listed the key considerations for the Digital Twin, we are ready to identify the candidates. Let’s take a scenario where the digital group is working with an aviation and power generation business. The aviation business builds jet engines for aircraft and provides services to airline customers. The power business builds generators that are used by electric utility companies. The utility company also buys long-term service contracts from the manufacturer. Based on these, the digital group is evaluating three candidates for its first Digital Twin. These are the following:

- Work with the aviation LOB to build the Digital Twin of the engine, E, that it manufactures.
- Work with the power LOB to build the Digital Twin of the gas generator, G, that it manufactures.
- Build a generic Digital Twin of a generator that is agnostic of the manufacturer. Since there are only a few engine providers for commercial aircraft, a generic jet engine model will not be very useful.

Now, let’s compare each of the three scenarios in the following table:

| Criteria | Twin of Engine, E | Twin of Gas Generator, G | Twin of Generic Gas Generator |
|---|---|---|---|
| First customer/user | Yes | Yes | Not yet |
| Reusability of twin | Low | Possible for other generators | High |
| Productivity/efficiency use case | Yes | Yes | No – external only |
| New digital revenues | Possible | Possible | Yes |
| Investment | Moderate | Moderate | High |

The preceding table shows how the digital group may list the objective criteria to help decide the pecking order for the Digital Twin identification for the starting point. The table shows that the twin of the engine, E, and gas generator, G, would primarily be internal use cases driven by the LOB. The investments can be justified by productivity gains obtained by the business by the use of the twins. However, when the use case for the twin involves only external customers, it may require a higher initial investment.

In the next section, let’s look at similar scenarios from the viewpoint of the LOB.

## Digital twin at the LOB

The LOB of an industrial conglomerate may look at the same scenario through their own lens before deciding to invest in the Digital Twin initiative. In this case, the LOB may look at three different criteria:

- The LOB’s own readiness for the Digital Twin initiative, whether is it complementary to its own business strategy or could be a possible distraction
- The ability to quantify productivity or efficiency delivered by the twin
- Technology direction

The LOB would look at its current business strategy and then see where the Digital Twin would be a good fit. For instance, aviation and power generation businesses would normally sell the physical asset once and then sell long-term services to their customers. In such cases, the LOB would invest in the Digital Twin, to help improve the predictive maintenance services. Since these contracts are long-term, the top-line revenue is not likely to change due to the introduction of the use of a Digital Twin for predictive maintenance. However, the delivery of the services and margin on them could be substantially improved by the use of a Digital Twin. Hence, it will go well with the business strategy for the LOB. A similar case study of the Digital Twin of an aircraft’s landing gear is available here: https://www.ge.com/digital/customers/predictive- insights-aid-aircraft-landing-gear-performance. This prototype of the Digital Twin was built by GE and Infosys and one of the authors of this book (Nath) was involved in it.

