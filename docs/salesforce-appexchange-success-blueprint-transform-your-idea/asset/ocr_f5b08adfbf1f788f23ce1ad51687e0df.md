you start writing a single line of code, engage in this critical phase of optimal pattern selection.

# Available technologies for outbound integrations

Salesforce offers a range of tools and methods to facilitate outbound integrations with external systems. In this section, we will explore the most popular and widely used outbound integration techniques provided by the Salesforce platform.

# Declarative with flows

Declarative outbound integrations in Salesforce are primarily achieved using Flow. Salesforce Flow is a powerful tool that allows users to automate business processes without writing any code. Within the realm of outbound integrations, Flow offers two main methods: outbound messages and external services.

Outbound messages are SOAP messages that Salesforce sends to external systems when specific criteria are met. They are designed to notify external systems of changes in Salesforce data. On the other hand, external services provide a way for flows to directly integrate with third-party APIs, supporting HTTP callouts with methods such as GET, POST, PUT, PATCH, and DELETE. When creating an external services package, developers can either add named credential components to the external service registration package or subscribers can establish a named credential in their org that matches the one indicated in the external service registration.

# Programmatic with Lightning Web Components