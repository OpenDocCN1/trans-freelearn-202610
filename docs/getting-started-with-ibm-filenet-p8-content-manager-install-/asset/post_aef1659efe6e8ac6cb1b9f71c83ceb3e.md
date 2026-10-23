
OK, so if marking sets are used to take away rights, how does that happen? If a user does not have the "use" right on a marking, then a set of rights is subtracted out of the effective access for the marked object. You define exactly what rights you want to remove by setting the Marking.ConstraintMask property. That's a set of access rights in the usual bitmask notation, but in this case the rights that are indicated will be subtracted from the marked object. If an object has more than one marking-controlled property, the calling user will have to pass the marking set check for each to get access to the object.

## A marking set example

You might find all the above facts a little confusing, so let's look at a concrete example. We assume you will do this as a thought experiment rather than actually going through the steps, so we don't describe them in detail:

- Define a flat marking set with three marking objects. The marking objects have the values Siss, Boom, and Bah or some values of your own choosing.
- Use the ACL on each marking object to define who has the "use" right for that particular marking. Give at least one user access (the "use" right) to a particular marking value and no access to the other marking values.
- For the constraint mask on each marking object, select all of the available rights. (There is no technical requirement to use the same constraint mask value for each marking object.) These are the things that will be denied to the calling user if they don't have "use" rights for that marking.
- On a document class, add a new string-valued property with a maximum length at least long enough to hold the longest marking value. In the FEM property template creation wizard, select the above marking set to use with that property.
- Make some document instances of that class and populate that string property on each one with one of the marking values. You can also make some document instances that leave that value unpopulated.

When a user tries to access a document, for retrieval or update, the CE goes through the following steps:

+ 1. It uses the document's ACL to compute the effective access rights for that user for that document.
+ 2. It then looks, in turn, at each string property controlled by a marking set. If the property has a value, the applicable marking object is consulted.
+ 3. If the user does not have the use right for that marking value, then the rights mentioned in that marking value's constraint mask are removed from the effective access rights for that user for that document. In our example, the constraint mask has all rights, so all rights would be removed from the effective access.
+ 4. After all marking set-controlled properties are considered, the effective access will have been updated to reflect mandatory access control on top of discretionary access control.
+ 5. The CE then makes its decision whether to allow the attempted operation.

Of course, this is a simplified description since the CE also has to take all of the other access control mechanisms into account.

You may be asking yourself what is so special about marking sets and mandatory access control. If you didn't have marking sets, couldn't you accomplish the same thing with some elaborate ACL on the document itself, perhaps along with other access control mechanisms? Quite often you could do that, but there is a qualitative difference.

With the discretionary access control mechanisms, the user can often add access to a document for someone else on a case by case basis. That's what is meant by discretionary. If they decide to do it, they can, and they don't have to consider any technical factor, like the other user's authority to access certain types of documents.

With mandatory access control in general and marking sets in particular, someone with the right to modify domain level objects can administratively set up the marking sets. It is that more privileged user who is, via the marking object ACL, effectively marking the users as having access to some things and not others. No matter what access someone else tries to grant to those users, the marking sets have the final say in letting the access proceed. It's extremely powerful, by design, although it can only be used to remove access rights.

## The final veto

It would be unusual, indeed, if you could not implement your access requirements using some or all of the bag of access control tricks described earlier in this section. If you do have that unusual situation, where your business logic for access control is elaborate, and the pre-defined mechanisms just aren't enough, then there is one more weapon you can use to deny access based on custom business logic.

In Chapter 7, we described the CE's event subsystem. As we said then, the main purpose for synchronous subscriptions is to provide an opportunity for the associated event handler to approve or veto a change to the triggering object. The event handler is custom code that you provide. It can implement whatever business logic you think is appropriate, including looking at other objects in CM. At the end of things, it simply decides, yes or no, whether it should allow the operation to proceed. If not, it throws an exception and the CE will roll back the transaction for that operation.

Synchronous event handlers can't be used to grant additional rights, but they can be used to deny rights that have been granted by all of the other CM authorization checks. The ability to use custom business logic is very powerful, but there are two important things to consider:

- There is a higher performance cost to doing this kind of thing via synchronous subscriptions compared to using the built-in access control mechanisms of the CE.
- There is no available event for retrieval operations, so you can't stop someone from reading property values or content via this mechanism.

## A hypothetical scenario

To illustrate most of the access control mechanisms described above, let's consider a somewhat complicated set of business requirements. This example is hypothetical, but it's not preposterous. It's actually a simplification of a CM security implementation question that came to our attention some time ago. We've changed the terminology and the motivations for the various things both to remove the specifics of the real scenario and to make the explanation clearer.

## The players

An enterprise has three major independently operating divisions: the Ninjas, the Pirates, and the Unicorns. There is also a corporate headquarters layer, HQ, that provides common services like human resources, finance, and so on.

## The business requirements

The divisions are very competitive with each other to the extent that it is necessary to have technical access control mechanisms in place to keep each division's nose out of other divisions' affairs. For routine matters, HQ is considered to be an honest broker and will not leak information from one division to another. That level of trust in HQ does not necessarily apply to more sensitive matters. (Hey, we said this was hypothetical. This kind of situation would surely never occur in any actual large enterprise. It certainly does not occur in your enterprise. We're thinking of something else entirely.)

Each division has a variety of projects of varying sensitivity. Here are the business rules we'll try to accommodate with our access control mechanisms. To simplify the description, we'll consider only read access to documents; other access rights, and other object types can be accommodated in straightforward extrapolations.

- An author of a document has access to his or her own documents unless another item in this list applies. For example, an author has access as long as the author stays in the same division, but it is not a requirement for the author to have continued access if the author changes divisions or moves to HQ. For convenience, we refer to this as *author access*.
- Typical access for run of the mill documents is that divisional personnel have access to documents within the same division. Personnel from other divisions do not have access to those documents. We refer to this requirement as *divisional restriction*.
- There are documents that deal with sensitive areas. Examples include human resources, finance, and medical information (about HQ or divisional personnel). Only specialists in those areas have access to those documents. We refer to this requirement as *functional restriction*.
- Each division has some especially sensitive projects. There is no overlap in special projects between divisions. Only personnel specifically authorized for each sensitive project can get access to related documents. Those personnel might be in the division or in HQ. We refer to this restriction as *special handling*.
- All HQ personnel generally have access to all divisional documents except functionally restricted and special handling documents.
- Functional area personnel, in HQ or in the same division, generally have access to functionally restricted documents of compatible types. For example, finance personnel from HQ have access to all finance documents. Finance personnel in the Unicorn division do not have access to HQ, Ninja division, or Pirate division finance documents.
- Likewise, there are HQ personnel who may have access to a divisional special handling project. The list of HQ personnel varies by project, and in some cases there are none at all.

Your first inclination after reading the above requirements might be to run screaming from the room or maybe just flip pages to the next chapter of this book. Stick with us, and we'll work through it.
