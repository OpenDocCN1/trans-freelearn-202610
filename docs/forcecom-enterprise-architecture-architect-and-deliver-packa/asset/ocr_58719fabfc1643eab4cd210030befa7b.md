> Note that not all platform APIs have an Apex variant that can be called directly, though this does not necessarily mean that they cannot be used.
> Apex supports making outbound HTTP callouts either via a WSDL or
> by directly creating and parsing the required requests or responses. A
> good example of this is the Metadata API, which can be used to develop
> Visualforce wizards to automate common setup tasks via Apex, as shown
> in an Apex example Ok to delete later in this chapter.

The following list details some of the main APIs that may be of use to your
Enterprise-level customers wishing to develop solutions around your application:

+   * **Enterprise API**: This SOAP API is dynamically generated based on the
+   objects present at the time in the subscriber org, meaning that any tool (such
+   as data-mapping tools) or developer consuming it will find the information
+   exposes more familiar and easier to access. One downside, however, is that
+   this can end up being quite a large API to work with, as Enterprise customer
+   orgs often have hundreds of objects and fields in them.
+   * **Partner API**: This is also a SOAP API; unlike the Enterprise API, it provides a
+   more generic CRUD-based API to access the record in the subscriber org. Its
+   name can be a bit misleading, as there is no restriction to non-partners using
+   it, and as it is lighter than the Enterprise API, it's often a first choice.
+   * **REST API**: This provides another CRUD style API, similar to the Partner
+   API, but in this case, it is based on the more popular REST technology
+   standard. It's often preferred when building mobile applications and is also
+   fast becoming popular as the default API for Java and .NET.
+   * **Metadata API and Tooling API**: These SOAP APIs provide a means to
+   automate many of the tasks performed under the **Setup** menu and those
+   performed by developers when editing code. While these might not be of
+   direct interest to those integrating with your application, as many of these
+   tasks are performed initially and then only rarely, it might be something
+   to consider using in tools you want to provide to keep the implementation
+   times for your application low.