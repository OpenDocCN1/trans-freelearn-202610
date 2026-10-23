Referential containment is accomplished by using a third object to link the folder and the contained object together. The CE has two different types of objects for that, and they are almost exclusively referred to by their acronyms because their names are quite impressive: **Referential Containment Relationship (RCR)** and **Dynamic Referential Containment Relationship (DRCR)**. Both have properties pointing to the folder (the Tail property) and the contained object (the Head property).

> The idea of the Head and Tail property names is that you can imagine an arrow drawn from the Folder to the contained object. The tail of the arrow is at the Folder, and the head of the arrow is at the contained object. Our own unscientific data suggests that that analogy is completely intuitive and easy to remember for about half of the people who come across Head and Tail. It's completely counter-intuitive and impossible to remember for the other half. If you're in that latter half, sorry about that!

You will almost always want to use a DRCR when filing documents, and most applications will make that assumption. An RCR's Head property can point to a Folder, Custom Object, or a specific Document version. Even if the document is versioned, the RCR will still point to that same specific Document version. The dynamic part of DRCR is that its Head property is automatically adjusted to always point to the current Document version in the Version Series.

Both RCR and DRCR are subclassable, so you can add additional properties relevant to application-defined relationship behaviors.

# Filing

Some specific facts about foldering:

+   - When a document is filed into a folder, it gets a **containment name**. Although the containment name typically means something useful for people about the document, it is technically just an arbitrary string label. You can reference a document by path by taking the folder's path and appending a forward slash and the document's containment name. That usually feels pretty natural, even to non-technical users.
+   - No two documents in the same folder can have the same containment name. We say that there is a **uniqueness constraint** on the containment names within a folder. That's pretty easy to understand when you consider referencing documents by path. How could you tell two of them apart if they had the same containment name?