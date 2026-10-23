# OpenStack is an API

One of the initial goals of OpenStack was to provide Application Program Interface (API) compatibility with the Amazon Web Service. As of the November 2014 user survey, 44% of production deployments were still using the EC2 Compatibility API to interact with the system. As the popularity of the platform has increased, the OpenStack API has become a de facto standard on its own. As such, many of the enterprise organizations that we've worked with to create OpenStack clouds are using them as an underlying Infrastructure as a Service layer for one or more Platform as a Service or Hybrid Cloud deployments.

Every feature or function of OpenStack is exposed in one of its REST APIs. There are command-line interfaces for OpenStack (legacy nova and the newer openstack common client) as well as a standard web interface (Horizon). However, most interactions between the components and end users happen over the API. This is advantageous for the following reasons:

+   - Everything in the system can be automated
+   - Integration with other systems is well defined
+   - Use cases can be clearly defined and automatically tested

![](bbox=[0.11, 0.59, 0.19, 0.68])

The APIs are well defined and versioned REST APIs, and there are native clients and SDKs for more than a dozen programming languages. For a full list of current SDKs, refer to http://api.openstack.org.

# OpenStack – an open source software project

OpenStack is an open source software project which has a huge number of contributors from a wide range of organizations. OpenStack was originally created by NASA and Rackspace. Rackspace is still a significant contributor to OpenStack, but these days contributions to the project come from a wide array of companies, including the traditional open source contributors (Red Hat, IBM, and HP) as well as companies which are dedicated entirely to OpenStack (Mirantis, and CloudBase). Contributions come in the form of drivers for particular pieces of infrastructure (that is, Cinder block storage drivers or Neutron SDN drivers), bug fixes, or new features in the core projects.