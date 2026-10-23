> The two-state nature of access rights makes it easy to compactly express them as a collection of bits in an integer field called a bitmask. Indeed, the CE does exactly that for efficiency. However, that is only interesting to application developers and students of the arcane. Most tools present the rights as individual checkboxes or similar UI items.

The set of possible rights varies by object type because the set of possible actions is different for different object types. A few access rights are available for all object types (for example, the right to read an object's property values), while others are more specialized (for example, the rights to create a major or minor version, which exist only for Document objects). As an illustration, there are eight individual rights defined for an Object Store, but there are fifteen individual rights defined for a Document object (plus two more that were used at one time but are not now obsolete).

# Levels

Although there is not a technical connection between individual access rights, it is certainly the case that some of them tend to travel together. For some use cases, you are likely to grant or deny a particular collection of rights to an object as a group to achieve some functional end. The CE APIs provide constants for many useful collections of rights, and many applications exploit this to display rights in various groupings. These groupings are called access levels. It is important to understand, however, that levels are not additional kinds of rights or a different mechanism. They are simply functionally interesting collections of existing access rights.

Earlier in this chapter, we showed a screenshot of the Security tab of a Properties panel from FEM. The access levels are used in FEM to create a series of radio buttons for quick selection. Here is a picture of the same sort of permissions as seen in XT:

![](bbox=[0.22, 0.6, 0.75, 0.8])