# Major CM Features

+   - Even if your business requirements are limited to a single application, a malicious attacker might be able to get access to or even create an alternative application to bypass your business logic. Applications lie outside the trust boundary. By that we mean that the CM server components do not trust any clients to have done anything correctly. Client logic is always assumed to be potentially faulty, whether by accident or by design. For example, if a particular range of values is specified in the CE metadata for a property, the CE server will always check incoming property updates against that constraint and will never assume the client has already done so.
+   - If there is server logic to accomplish a particular business objective, then it is typically more expensive in development, test, and support time to redundantly implement similar logic in applications.

We're obviously making the case for exploiting server-side mechanisms wherever possible. CM server components have rich palettes of server-side mechanisms already available. For some examples, look at the section on defining custom properties earlier in this chapter.

There are cases where you would like to extend the server-side functionality so that you can use your own custom business logic but still get the advantages of a server-side implementation. The CE event subsystem fulfills this role, and it is also one of the central features in what CM product documentation calls *active content*. In this section, we describe event handling via subscriptions. Because the mechanism for audit logging is so closely related, conceptually and mechanically, we also describe that in this section.

# What is an event?

An event is simply something that happens to an object in a CE repository. Examples include creation of an object, checkout of a Document, and filing an object into a Folder. Most events are update events, while the only non-updates event are the retrieval of an object or the retrieval of Document or Annotation content. An event has a triggering object (the object which caused the event to fire), and various types of actions may be triggered when the event is fired.

+   - For update events, event handlers may be executed to perform site-defined activities
+   - For update events and object and content retrievals, audit logging records may be written
+   - For document lifecycle promotion or demotion, a lifecycle event handler may be executed