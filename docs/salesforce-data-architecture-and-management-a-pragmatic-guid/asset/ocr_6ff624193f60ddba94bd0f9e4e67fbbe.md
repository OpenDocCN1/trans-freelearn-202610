# # Monitoring performance

In this section, we will discuss the tools that we can use to monitor our Salesforce orgs proactively. Remember our discussion from *Chapter 3, Understanding Data Management*: preventing a problem from happening costs $1 compared to remediating it once it has materialized, which costs $10. If the remediation doesn't happen in time and a failure happens, it could cost $100 to fix. In the next section, we will discuss what to monitor, and in the subsequent sections, we'll look at some of the tools that are available for monitoring your Salesforce org.

# ## What to monitor?

There are tens of benchmarks to measure, but are there a few that we can measure that will provide the most value to us? The short answer is yes; you can track and monitor performance on the following metrics. For some of these, you will need to consult your networking team to suggest the best tools available for the job. Be prepared to explain how Salesforce works, its multitenant architecture, and the fact that organizations don't get direct access to the underlying databases and application servers:

+   * **Page load time**: This is an obvious one as usually, users will start to complain as they see their pages take longer to load. LEX pages are made up of multiple components and when reviewing the results, you would want to see the component load sequencing along with the load time. This is important from a diagnostic perspective because then you can assess which component is causing the issue. The issue can be any number of things, such as network latency, third-party host issues, or CDN issues.

You can run automated tests for Salesforce pages to record the page load time (EPT in LEX). There are tools now that can log them, send alerts if they are below a certain threshold, and run scheduled reports for trend analysis. These tests can provide detailed data on the execution time for each component, the sequences in which the component was loaded, and other details about the component.

+   * **Network**: These tests provide details about the performance of the network layer. These details include latency, packet loss, the number of hops, and other data such as communication between network devices as the packet travels from the source (client's web page) to the destination, that is, the Salesforce servers.