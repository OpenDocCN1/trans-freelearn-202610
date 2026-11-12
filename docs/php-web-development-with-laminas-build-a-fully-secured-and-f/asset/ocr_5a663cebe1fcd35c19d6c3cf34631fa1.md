> **RSS reference**
> You can find documentation about RSS at https://validator.w3.org/feed/docs/rss2.html.

# ## Hydrator

In an object-oriented application, there are some frequent operations, such as the following:

+   - Hydrating objects (populating them with a dataset)
+   - Extracting data from objects

The `laminas-hydrator` component provides hydrating and extracting functionalities. This component requires the following from the classes to be hydrated and extracted: they must implement the `exchangeArray` and `getArrayCopy` methods, the same required methods for mapped models, as we saw in *Chapter 13, Tips and Tricks*.

# ## JSON-RPC server

JSON is the de facto standard for REST APIs. But these aren't my words; they're the words of Balaji Varanasi and Sudha Belida in their book, *Spring REST*, from *Apress*, published in 2015. They are right. Nobody will expose API data in a format other than JSON today, except if a constraint requires it.

The `laminas-json-server` component enables you to expose PHP classes as services. Using PHP DocBlocks, the `laminas-json-server` component allows remote procedure calls to PHP functions and methods.

> **JSON-RPC reference**
> You can find the JSON-RPC specification at https://www.jsonrpc.org/specification.

# ## LDAP

The **Lightweight Directory Access Protocol** (**LDAP**) is an open standard protocol for managing directory services. There is an open source implementation of LDAP named OpenLDAP available in several Linux distributions. Many companies sell LDAP services as RHDS by Red Hat or Azure Active Directory by Microsoft. You can find LDAP implementations in most cloud providers.

It is useful to have a component that makes it easy to manage LDAP operations in PHP applications. This is exactly what `laminas-ldap` does.