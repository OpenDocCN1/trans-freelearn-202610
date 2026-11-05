# 3

# BPEL Process Monitoring

It is important to understand the complexity of a BPEL process that creates the additional need for active operational monitoring. BPEL processes are the new generation of workflow. They are SOA enabled, which means they orchestrate interactions between different systems using a common **Web Service Invocation Framework (WSIF)**. WSIF enables BPEL to build a façade to interact with any system that is Web service enabled. The Web service interactions are called "partner links" in the BPEL framework. This is quite powerful for orchestrating complex business processes that involve new and legacy systems. It provides IT with an automated framework for enterprise-wide business processes. Monitoring BPEL processes is important for business analysts and administrators to maintain business visibility and resolve problems quickly. This chapter talks about viewing deployed BPEL processes and their constituent partner links. Further, monitoring of BPEL processes and partner links using a combination of metrics and synthetic tests is covered.

Grid Control provides means to monitor critical BPEL processes, partner links, and Web services through service tests to determine availability and response time. Grid Control also provides a means of measuring critical metrics for actual requests initiated against each of the Web services deployed on the container. With a combined end-user and request perspective, Grid Control can determine the service availability of all monitored services.

+   - Challenges
+   - Solution
+   - Step-by-step exercises:
+     - Navigating to the BPEL PM target home page
+     - Navigating to the BPEL process home page
+     - Creating the BPEL process aggregate service
+     - Creating a SOAP test to monitor a partner link
+     - Creating a SOAP test to monitor a BPEL process
+     - Testing the SOAP tests