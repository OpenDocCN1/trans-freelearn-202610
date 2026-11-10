# 12

# Creating Lifecycles, Alias Sets, and Permission Sets

In the previous chapter we created a custom object type and some attributes for the News Article object. Let us now design a custom Lifecycle for the News Article object so that when an instance (or content object) is created from News Article object type, it can be moved across the various Lifecycle stages such as Staging (or Review), Approval, etc. before taking the content live. A Lifecycle object in Documentum is associated with some other related objects such as Alias Sets, Permission Sets (ACLs), etc. and it is advisable to quickly go through the introductory chapters to flip through the basics we discussed earlier.

Broadly speaking, while designing Lifecycles, you need to bear in mind three important components:

+   - Alias Set(s) for the Lifecycle
+   - Permission Sets (ACLs) for various stages in the Lifecycle flow
+   - Actual Lifecycle design such as stages, labels, any scripts for pre or post processing, etc.

We will cover each of these in this chapter, by taking very simple examples so that you get a certain level of understanding and comfort that can be further extended and enriched while designing real-time applications in software projects.

# 12.1 Alias Sets in a Lifecycle

An Alias Set in the context of a Lifecycle is a group of Aliases that make a Lifecycle portable across numerous environments. An Alias is nothing but a placeholder or a symbolic name for some actual value in a given context called an Alias Name. An Alias Value is the actual value for the Alias Name. A pair of Alias Name and Value constitutes an Alias and a group of such Aliases constitutes an Alias Set.

Imagine that a content object for a News Article was created by a content author and reviewed by a content manager. In a given department, the content author could be Mr. X and the content manager could be Ms. Y. In another department, the content authors and managers could be different persons and not necessarily Mr. X and Ms. Y.