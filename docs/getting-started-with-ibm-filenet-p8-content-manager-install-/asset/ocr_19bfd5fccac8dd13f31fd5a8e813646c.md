The more popular of the two mechanisms just described is the versioning-related feature. You could achieve the effect of either type of security template by using a combination of other CM features, but it's typically more performance-efficient and simpler application logic to use the security policy features if they fit your use case.

# Document lifecycle policy

Document lifecycle policies have a feature quite similar to that of security policies, but the mechanics and terminology are slightly different. The lifecycle feature in general is described in Chapter 7. Here, we concern ourselves with just the feature for automatic security changes.

A document has a DocumentLifecyclePolicy property whose value is a Document Lifecycle Policy object. A document lifecycle policy object has a list of document state objects. Besides giving the lifecycle state details, the document state object also has an optional list of template permissions. The template permissions collection is just a list of ACEs.

Here's how this is used: When a document transitions to a particular lifecycle state, the list of template permissions for that state is copied to the document. Existing ACEs that came from a template are first removed. A boolean property on the document lifecycle policy object determines whether any direct/default ACEs are also removed.

You can see that this is quite similar in concept to the security template mechanism described above. In the lifecycle case, an application triggers the document lifecycle state transition, but the CE automatically updates the security based on the document lifecycle policy and document state objects.

# Dynamic security inheritance

Security inheritance is a powerful feature that can greatly simplify the administration of access control in your CM environment. We've mentioned security inheritance in passing before, but now we'll cover it in more detail.

> In CE, the feature is called dynamic security inheritance to emphasize the fact that it is evaluated dynamically (and instantaneously). Some earlier versions of CE used background copying of ACEs from parent to child to simulate inheritance. Although the functional effect is the same in terms of the security you would actually see on child objects, the mechanics had some occasional undesirable side-effects. Today's CE only uses true inheritance – and does not use background copying of ACEs wherever something is described as security inheritance.