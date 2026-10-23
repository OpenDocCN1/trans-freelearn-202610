
FEM and XT use somewhat different terminology for combinations of rights. This is due mainly to the different audiences for the two tools.

If you are developing an application that displays or manipulates security details, you may or may not find the levels (defined in API class AccessLevel) to be interesting for your use case. If they're not helpful, don't use them.

## Unused bits

Now that you know that access rights are stored as a collection of bits and are manipulated programmatically as bits, you may find yourself looking more deeply into the defined access rights bits. The bits are stored as a 32-bit integer, but not all of the bit positions have defined meanings at this time. If you were to use some of those otherwise unallocated bits for your own purposes, that would not interfere with the CE's access check calculations. Of course, most tools won't even give you a way to set those bits, but you could do it programmatically. That is a bad idea, and here are some reasons why you shouldn't do it:

- Other applications shouldn't care about the bits you are using, but neither are they guaranteed to respect them. They might just clear them on the grounds that they know they are not defined for anything.
- Some applications have both simplified and detailed views of security. The simplified views are based on well-known combinations of access rights that the application thinks are meaningful. Setting unallocated bits is almost certainly going to cause those applications to abandon the simple view and always show the detailed view.
- Access rights bits don't change very often, but it's entirely possible that the next release of CM will define a meaning for the very bits that you are using. All unallocated bits should be considered reserved for future use by the system.

If it's such a bad idea, why would you want to do it in the first place? You might be thinking it's a handy place to do bookkeeping for your own private access rights. Maybe a synchronous event handler of yours would check those bits as part of its decision logic for whether to allow an operation. That's not a bad idea; it's just a bad idea to use unallocated access rights bits. You'll have to find some more traditional place for keeping track of such things.

## Implicit rights

In addition to the rights conferred via the ACL mechanism, there are two special cases where rights are implicitly given to calling users.

### Object Owner

An object has an Owner property. It is usually the case that the user who created the object is also the owner, but there are other possibilities. There is a particular access right called WRITE_OWNER (but often informally called "take ownership") that means someone (or some group) is authorized to assume ownership of the object.

The owner of an object is always allowed to update the object's ACL (even if not explicitly granted the WRITE_ACL right). Because of that, the owner of an object is indirectly given all other available rights for that object. We say it's indirect because the ACL must actually be modified to confer those other rights.

Suppose you wanted to eliminate this exception and limit actions to explicitly granted rights. (Records Management has this requirement, for example.) It's obvious how to make sure nobody has the WRITE_OWNER access right. What about the owner of the object? It's possible to set the value of the Owner property to null, in which case there is no owner, and there is nobody who has the implicitly-granted owner rights. A typical user with the WRITE_OWNER access right can only set the Owner property to his or her own identity. To set it to null requires an additional privilege discussed in the next section.

### Object Store administrator

There actually is no such thing in the eyes of the CE as an Object Store administrator. There are only authenticated users with or without various access rights. Tools like FEM have a working definition for an Object Store administrator, which is a user with full access rights to the Object Store object. That's mainly because routine matters in an administrator tool like FEM can become pretty sticky, pretty fast if the user doesn't have the rights to do various things.

However, there is one specific access right that, more than any other, feels like "the administrator bit". That access right is called WRITE_ANY_OWNER and is available only for Object Store objects. A user with this access right is allowed to update the Owner property of any object in the Object Store with any legal value, including null.

You can probably guess the next part: users with WRITE_ANY_OWNER rights on the Object Store are treated as if they also have WRITE_OWNER rights on each object, which means they have the right to modify each object's ACL; this means they can do pretty much whatever they want to those objects. It should be clear now why having the WRITE_ANY_OWNER right is often thought of as being the same as being an administrator.

Interestingly, the Object Store itself does not have a WRITE_OWNER access right. That's because it doesn't have an Owner property. It does have a Creator property, but that confers no special privileges.

### Special query right

To see an object, which really means to view an object's properties, users need READ rights to that object. Otherwise, the very existence of the object is hidden from them. Users with WRITE_OWNER or WRITE_ANY_OWNER could update the object's ACL and grant themselves READ access. Before they could change the ACL, they would have to get a reference to the object. If they don't already have READ access, it can be difficult to get the object reference. The object will not show up in ordinary browsing.

The situation of owning an object but not having READ access is called the "lost object problem". It usually comes about by accident, and usually everyone has lost READ access to the object. The object is effectively invisible, but it still exists. That can be more than just an annoyance. For example, the lost object might still prevent deletion of other objects due to referential integrity checks in the CE.

To provide a solution to the lost object problem, there is a special case that happens only when query results are being returned. Objects for which the caller does not have READ access are normally silently excluded from the query results. However, they are not excluded if the calling user has WRITE_OWNER or WRITE_ANY_OWNER access. The idea is that lost objects could be located via such a query, thereby providing the needed reference so that the ACL could be updated (since those same calling users would also have implicit WRITE_ACL rights).

## Extra access requirements

Beyond ACEs and the various mechanisms for manipulating them, manually and automatically, there are a couple of common cases where you might want some additional control. CM has specific mechanisms for those cases. The mechanisms are both implementations of additional restrictions on setting specific properties on an object, and so they are configured in the property definition for the property in question.

### Modification Access Required

Sometimes you would like to put extra protection on a specific property so that even users with the right to update properties generally on an object still need a bit more access to be able to modify that particular property. The CE has ACLs on objects, not on properties of objects. A feature called **Modification Access Required (MAR)** lets you use the object ACL to protect a specific property.

The property PropertyDefinition.ModificationAccessRequired is an integer value that defines, in the usual bitmask representation, the access rights to the object that a user must have in order to be allowed to modify that property. In the usual case, the value is zero (in which case updates are governed by the object ACL), but you can define additional required access rights for your custom properties. For example, you might require that a calling user have the right to delete the object before being allowed to update a particular property. That doesn't mean that they must be actually deleting the object; it merely means they must have the right to delete it. You can view or modify the MAR value in FEM by editing the Property Definition for the property. For example, here is the MAR for the inherited property WjcWorkOfLiterature.Permissions. Because it's a system property, you can't modify the MAR. It's not surprising to find out that the Modify permissions right (WRITE_ACL) is required.

![](img/2295191c450a0c86b2d82df6cec04526_268_0.png)

When an attempt is made to update property values on an object, the CE first computes the effective access that the calling user has for that object. Assuming all the object-level access checks pass, the CE looks at the modification access required for each property being updated. The calling user must also have all of those rights, or the update will fail.

### Target Access Required

There are many ways to form relationships between objects in CM. Most of them were described in Chapter 7, Major CM Features. We usually think of a relationship as updating a property on one object to point to another object, and we usually think about the right of the caller to update the property on the pointing object. It is sometimes useful to let the pointed-to object have some say in whether another object can point to it. A feature called Target Access Required (TAR) implements that.
