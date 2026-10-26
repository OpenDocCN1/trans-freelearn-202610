# 6
Communication
Pitfalls and
Prevention

Microservice architectures are implementations of distributed systems, where microservices are designed to be fine-grained, loosely coupled, and autonomous in order to deliver a business capability. These microservices communicate with each other across process boundaries, which are hosted across the network, to address different business use cases. The network communication is unreliable; hence, microservices should incorporate the necessary logic to enable effective communication across service boundaries to build resilience. The challenges of designing a distributed system are well known. They are described by Peter Deutch as assumptions developers make while they transition from a single-process application to a distributed system. These assumptions are defined as fallacies of distributed computing. The fallacies are as follows:

+   - The network is reliable.
+   - Latency is zero.
+   - Bandwidth is infinite.
+   - The network is secure.