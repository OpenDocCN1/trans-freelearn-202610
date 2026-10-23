+   - Many directory servers are supported, including IBM Tivoli Directory Server, Novell eDirectory, Sun Java Directory Server, Oracle Internet Directory, and, of course, Microsoft Active Directory (both the full version and AD Lightweight Directory Services). The list tends to evolve over time, and you should also check for specific release numbers.
+   - CM supports a variety of storage technologies and vendors for content storage, including EMC Centera, NetApp Snaplock, and IBM System Storage N Series. That list also evolves over time.

# Integrating content with workflow

It's extremely common to use workflows as part of ECM. Whether you use the PE server bundled with CM or a more generally licensed BPM installation, you will want to consider the relative placement of these two engines when deploying your environment. We described workflow integration with examples from XT in Chapter 6, End User Tools and Tasks.

+   - The PE server need not be able to reach all CE servers, but it must be able to connect to at least one CE server in order to retrieve and update objects carried as attachments to workflows. For example, an update to a workflow property in a step processor may trigger a corresponding update to one or more document properties in CE.
+   - The CE must be able to connect to the PE in order to launch workflows in response to update events for documents and other objects. For example, the check-in of a new major version of a document may trigger an approval or publication workflow.
+   - Applications, particularly those involved with workflow step processing, will often need to connect to CE servers and PE servers.

# Content storage

You have several choices when it comes time to decide where to store your document content within CM. Properties for documents and other objects are always stored in the Object Store database, but the actual content bits for a document or annotation are stored according to configured storage areas and storage policies, as described in Chapter 7, Major CM Features. No matter where you choose to put the content, you will not be able to directly access it from your applications. Your applications will always go through a CE server to get the content as a stream of bytes. This makes the actual storage logistics transparent to applications and also ensures that CM security is not bypassed.