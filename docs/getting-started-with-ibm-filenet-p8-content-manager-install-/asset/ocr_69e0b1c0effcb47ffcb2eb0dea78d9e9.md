For the purposes of distributed deployments, we are generally interested in the sites. It's common to describe a distributed deployment in terms of a central site and one or more remote sites. However, the CE itself does not have the concept of a central site. For the actual mechanics of CE behavior, it makes more sense to talk about the local site and a remote site. Which one is which really depends on where the Object Store or other resource lives.

# PE considerations

For PE, the objects involved are very dynamic. Because of the high rate of change, there is very limited caching of PE objects. PE activity tends to be database intensive, so it makes sense to locate the PE server near the PE database. That should be within the same physical datacenter.

The PE and CE converse for a limited number of things: PE's delegated authentication (typically lightweight in terms of network traffic), retrievals and updates of workflow attachments (can be heavyweight if content fetches are involved), and CE calls to PE to launch workflows. It is not a requirement that the PE and CE have the same physical location, but performance will be improved if they do.

# CSE considerations

The Content Search Engine (CSE) does full-text indexing and responds to full-text queries. Calling applications do not connect directly to the CSE. The CSE is accessed only through a CE server that is making CSE API calls. CSE must see its storage as being local to the CSE server even if that storage resides on some kind of network share.

It's probably easiest to think of the CSE as just another kind of database server. Since all CE servers must be able to talk to all CSE servers, it makes sense to centralize CSE servers unless you're lucky enough that your content naturally partitions for particular sites.