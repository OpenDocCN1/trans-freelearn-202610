However, when you are building a similar service using the same technology, you can sometimes reuse the code or framework to reduce your development efforts. For that reason, in some cases, reusing the existing code seems to be a good practice. Here are some important aspects to consider when evaluating this factor:

+   - Are you maintaining a service template or utilizing a framework to kick start new service development?
+   - Do you have a single code base for microservices?
+   - Are you isolating dependencies?
+   - Are you externalizing configuration?
+   - Is sensitive configuration securely stored?
+   - Are you containerizing your services?
+   - Have you used a cache where appropriate?
+   - Do you know your data consistency requirements?

# Capabilities of the hosting platform

One of the main benefits of building a microservices architecture is to embrace the easy scalability of services. Containerizing a monolithic application is a challenging task. However, if the application is decomposed into fine-grained services, these can be easily containerized and scaled out based on the need. Discussing the capabilities of the hosting platform for a microservices architecture is an important factor. Here are some aspects that you can discuss to evaluate this factor:

+   - Which hosting platform are you using to deploy your services?
+   - Is the hosting platform scalable?
+   - Does the hosting platform support auto-scaling?
+   - How much time is needed to scale the hosting platform?
+   - Is there any proper SLA provided by the vendor for the hosting platform?
+   - Is there any DR site for the hosting platform?