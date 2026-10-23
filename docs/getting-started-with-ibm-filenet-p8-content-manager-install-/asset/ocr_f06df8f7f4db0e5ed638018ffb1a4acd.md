Because it is a rich topic, we are saving most of the discussion of security for Chapter 8, Security Features and Planning which is entirely devoted to security concepts.

# Documents

No questions asked, the single most fundamental feature of any content management system is to be able to, well, manage content. Like many other areas of modern software practice, we use the acronym CRUD — create, retrieve, update, delete — to describe interactions with documents. It turns out that many of the features we describe for documents are also features of other object types in the CE, but we'll get to that a bit later in this chapter.

A document in the CE is an independently addressable object that has properties and optional content. Although it's not usually very interesting to create documents without content, it can be done and is sometimes used for things like content that will arrive later.

You may already be familiar with the notion of documents with associated internal properties. Most office productivity applications apply internal properties to the documents they create. These can include things like the author's identity, the time spent editing the document, a copyright notice, or any number of other things. The CE's properties are not directly associated with those internal properties, but it's not unusual for an application adding a document to a CM repository to extract those internal properties from the document and use them to populate similar CE properties. Although the roles for the properties are analogous, you will see that the CE's model for properties is quite a bit richer.

# Content

A document can contain any number of content elements. It's easy to understand a content element as being "the actual bits" of a document. If you have a text file, a PDF, a word processor document, or an HTML page on disk, that entire file is the thing we are talking about. The CE stores the file verbatim. No transformation or interpretation is done on the file's contents. When someone later retrieves that document, the content will be 100%, bit-for-bit identical to the content that was uploaded.

One of the key features of the CE is that document content is immutable once it's uploaded to the repository. While a document is checked out (see the Versioning section later), you can replace the content as often as you would like, but there is still no such thing as modifying a particular content element. Once a document is checked in, the list of content elements is frozen, and none can be added, deleted, or replaced.