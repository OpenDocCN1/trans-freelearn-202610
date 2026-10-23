The solution we will use is to put the total count and list of borrowers on the RQ document as described above. We'll then use a satellite object to synchronously handle the events and update the RQ document properties. There are many implementation choices for this satellite object (by which we merely mean that it is secondary to the "real" object, the RQ document), but we like to use the handy built-in Annotation class when we can. It has instances with some favorable characteristics:

+   - An annotation is associated with a particular object called the annotated object. The annotation cannot be moved to another object (because the AnnotatedObject property can only be set at the time the annotation is created).
+   - The Annotation base class has only a few properties, most of which we do not care about. If we have to, we can give access to all of the annotation properties without worrying about strange side effects.
+   - An annotation is automatically deleted when its annotated object is deleted. That makes it easy to avoid referential integrity problems.

Naturally, if we didn't already have the Annotation class available, we could construct all of those same things from scratch using, for example, a CustomObject subclass and standard CE features.

We will define a custom subclass of Annotation called WjcRestrictedQuantitySatellite, display name Restricted Quantity Satellite. We don't need any custom properties for this annotation subclass since we're only using it in this satellite object capacity. In fact, the use of a custom subclass is only to distinguish the particular annotation we will be using from other annotations that may happen to be associated with the document.

The act of borrowing or returning a copy is implemented as updating the WjcRestrictedQuantityBorrowers list to add or remove, respectively, the calling user's identity. The business logic for this is implemented in handler for a WjcRestrictedQuantityBorrowReturnEvent raised on the annotation. The handler for that event (DuckRQB, described more fully below) performs the following actions:

+   - If the annotated document is not an RQ document, no action is taken.
+   - If the calling user's identity is on the list of borrowers, the operation is a return. The user's identity will be removed from the list of borrowers.
+   - Otherwise, it is a borrow operation. The user's identity will be added to the list of borrowers if there is at least one copy available for borrowing. If there is no copy available, the handler will throw an exception.