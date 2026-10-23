# Roles and adapters

The usual cases for security inheritance are along the lines of "I want the security for this folder to apply to anything I file into this folder" or "I want this document's annotations to have the same security as the document". In other words, the first inclination to use security inheritance is for a rather static set of cases with the inheritance along functional lines. The dynamic aspect is also available, even in those use cases. If you update the security of the folder or the document, in the first and second examples, respectively, the inheriting objects automatically and instantly pick up the inheritable part of the change.

More complicated arrangements are possible, as we will illustrate with the following example.

# Project team example

What if you had a collection of objects that had a relationship in business logic but no obvious low-level technical relationship? It might be some folders that don't have a common parent folder, or it might be some documents or custom objects that are filed in different folders or perhaps no folders at all.

There are very few restrictions on OVPs, including those that cause security inheritance. You can make pointed-to objects whose whole purpose is to define inheritable ACEs. Many unrelated objects could point to a single instance of that object. Because of the way they are used, and which we are about to describe, they are commonly called role objects.

Suppose, for example, that you had a repository of documents and folders that belonged to various project teams, and your security requirement is that project team members should only have access to their own artifacts. You could create a series of role objects with one role object for each project team. ACEs on each role object would grant rights to members of the project team and would be inheritable to unlimited depth. By simply assigning a role object to an inheritance-defining property on each document, all project team members would have access due to inherited security. If the document had no ACEs of its own, the implicit deny would prevent access by anyone not on the project team.