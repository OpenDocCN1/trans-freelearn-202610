+   - **Relationship**: This is represented by a line joining the entities involved in the relationship and optionally the name of the relationship close to the line

![](bbox=[0.12, 0.16, 0.85, 0.33])

Figure 3.4: E-R notation basics

In the previous figure, we have illustrated two of the most used notations at a level of detail that you generally use to describe a conceptual model. In this example, in fact, we do not show keys and attributes, but you have already seen an example with attributes and keys in the example describing the logical models in the previous section of this chapter.

This example also illustrates cardinality, which is another core aspect of an E-R model, which we will describe in the following section.

# Cardinality

The **cardinality** of a relationship indicates how many instances of one entity on one side of the relationship participate in the relationship with how many instances of the entity on the other side.

Let's see the possible cases using the crow's foot notation:

+   - **One-to-one cardinality**: This is quite a rare case where only one instance of each entity participates on each side.

An example is the relationship **Person has Passport**: one specific person has zero or one (active) passports, and one passport belongs to exactly one person. Note that if the **Passport** entity semantic was not intended to record only the current active passport, we would have a one-to-many relationship, as a person can have multiple passports over their life. Also, some countries allow having more than one active passport at the same time, so it is really important to correctly understand and describe the semantics of each entity in the domain.