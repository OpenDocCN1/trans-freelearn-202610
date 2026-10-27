Managing intentions within the UI is quite simple either by creating a new intention or editing an existing intention. Every service within the Consul mesh is provided as a source and destination, and we can either choose to allow, deny, or create an **Application Aware** rule:

![](bbox=[0.137, 0.194, 0.866, 0.521])

*Figure 6.10 – Editing intentions within the UI*

The **Application Aware** rules add complexity through the use of explicit permissions. Recall that layer 7 intentions provide additional functionality to define intentions based on the path of the URL or even the headers in the request.

Up until now, every scenario that we've been discussing has been based on the event that every application and service exists within the service mesh. What can we do about services that reside outside of the mesh? Well, I'm glad you asked, as that question provides an excellent gateway to the next topic.