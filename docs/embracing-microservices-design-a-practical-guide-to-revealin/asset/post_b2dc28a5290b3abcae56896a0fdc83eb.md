
## Chapter 6: Communication Pitfalls and Prevention

1. In synchronous communication, the client sends a request and waits for a response from the service. Synchronous communication can be blocking or non-blocking, but the client can only continue further processing once the response is received from the service. In asynchronous communication, the client sends a message to the message broker that acts as middleware between the client and service. Once a message is published to the message broker, the client will continue processing without waiting for the response. The service can subscribe for specific messages to the message broker and place responses back in the message broker after processing. Both the client and service are unaware of each other and can continue to evolve without any dependency.

2. Direct client-to-microservice communication introduces tight coupling between the client and microservice, where clients need to be aware of how different microservices can work together to achieve a business use case, which results in implementing service orchestration by the client application. Furthermore, calling each microservice from the client adds latency and affects the user experience and mobility.

3. When multiple microservices call each other repeatedly in a long chain, there is potential that a microservice may take more time to respond than expected, which can result in timeouts. These timeouts will initiate additional retry requests with an expectation that the operation may succeed and the flood of these retries will eventually make the system unusable. The situation is known as a retry storm.

4. There are four different approaches to HTTP/REST API versioning, as mentioned next:
   a) URI path versioning
   b) Query parameter versioning
   c) Content-type versioning
   d) Custom header versioning

5. A service mesh provides an abstraction layer on top of microservices to provide capabilities such as traffic management, security, policy, identity, and observability for your microservices. The idea behind the service mesh is to decouple these concerns from the implementation of microservices and handle them in a consistent manner across the microservices architecture.

![](img/e6276d4d880001c04e81cb4db4898f1c_289_0.png)

Packt.com

Subscribe to our online digital library for full access to over 7,000 books and videos, as well as industry leading tools to help you plan your personal development and advance your career. For more information, please visit our website.

## Why subscribe?

- Spend less time learning and more time coding with practical eBooks and Videos from over 4,000 industry professionals
- Improve your learning with Skill Plans built especially for you
- Get a free eBook or video every month
- Fully searchable for easy access to vital information
- Copy and paste, print, and bookmark content

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at packt.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at customercare@packtpub.com for more details.

At www.packt.com, you can also read a collection of free technical articles, sign up for a range of free newsletters, and receive exclusive discounts and offers on Packt books and eBooks.

## Other Books You May Enjoy

If you enjoyed this book, you may be interested in these other books by Packt:

![](img/e6276d4d880001c04e81cb4db4898f1c_290_0.png)

## Building Microservices with Micronaut

Nirmal Singh, Zack Dawood

ISBN: 9781800564237

- Understand why Micronaut is best suited for building microservices
- Build web endpoints and services in the Micronaut framework
- Safeguard microservices using Session, JWT, and OAuth in Micronaut
- Get to grips with event-driven architecture in Micronaut
- Discover how to automate testing at various levels using built-in tools and testing frameworks
- Deploy your microservices to containers and cloud platforms
- Become well-versed with distributed logging, tracing, and monitoring in Micronaut
- Get hands-on with the IoT using Alexa and Micronaut

![](img/e6276d4d880001c04e81cb4db4898f1c_291_0.png)

## Python Microservices Development - Second Edition

Simon Fraser, Tarek Ziadé

ISBN: 9781801076302

- Explore what microservices are and how to design them
- Configure and package your code according to modern best practices
- Identify a component of a larger service that can be turned into a microservice
- Handle more incoming requests, more effectively
- Protect your application with a proxy or firewall
- Use Kubernetes and containers to deploy a microservice
- Make changes to an API provided by a microservice safely and keep things working
- Identify the factors to look for to get started with an unfamiliar cloud provider

## Index

## Symbols

.NET 205
.NET Core 205

## A

abstracting microservices 85
A/B testing deployment strategy 216
actors 88
Advanced Messaging Queuing Protocol (AMQP) 37
aggregates 61-65, 142
aggregator microservice 76
aligned teams 65, 66
anti-corruption layer 60
Apache JMeter 246
API composition pattern 147, 148
API Gateway 199
API gateway microservice
about 76
and aggregator microservice 78
with cross-service queries 77
API gateway pattern
about 16, 157, 158
advantages 37

used, for providing message transformation 194-196
API microservices
versus core microservices 106
API versioning
reference link 176
application monitoring, elements
logs 206
metrics 206
traces 206
application programming interfaces (APIs) 256
application versioning 171
approvals
using 227
architects 18
architectural decisions
managing 251
asynchronous communication 153
asynchronous message-based communication 154
atomic, consistent, isolated, and durable (ACID) 142, 143
atomicity 142
AutoFixture 238
automation 7, 239

autonomous 35
autonomy 7
availability 115, 162
Azure Active Directory 200
Azure API Management
about 189, 199
reference link 196
Azure Application Insights
reference link 208
Azure App Service deployment
slot configuration
reference link 215
Azure DevOps Services
URL 109
Azure Front Door 199
Azure Key Vault
about 199
best practices 200
reference link 200
Azure Kubernetes Service (AKS) 91
Azure RedHat OpenShift (ARO) 91
Azure Resource Manager (ARM) 222

## B

backing service 26
behavior-driven deployment (BDD) 240
Bicep
about 223
URL 223
bindings
input bindings 88
output bindings 88
black box environment 241
blue/green deployment strategy 217
bounded context 58
brownfield application
refactoring, activities 105

build stage 26
business
area, changes 254
core priorities, defining for 19-21
core priorities, identifying 250
business domain
knowledge 102, 103
business leaders 17
business logic layer 70

## C

canary deployment strategy 214, 215
capability/feature matrix 84
cart aggregator microservice
AddItems 74
CalculateCartValue 75
CleanCart 75
cart microservice 72
centralized monitoring
implementing 206-208
chaotic context
about 49
approach 49
risk 49
chatty services
avoiding 107
choreography
benefits 145
challenges 145
choreography implementation
of saga 144
circuit breaker pattern
about 83, 118, 164
closed state 118
half open state 118
open state 118
using 119, 190

circuit breakers 166
client-side service discovery patterns
implementing 201, 202
client-to-microservice
communication 156, 157
cloud-native applications
elements, to consider 104
cloud-native approach 21, 91, 92
Cloud-Native Computing
Foundation (CNCF) 221
cloud-native services 104
command 153
Command and Query Responsibility Segregation (CQRS)
about 64, 133, 169
problem, solving 139
solution 140
command-query separation (CQS) 133
communication 36
communication protocol
selecting 257
complex adaptive systems
about 43
relationships 44
complex context
about 47
approach 47
risk 48
complicated context
about 46
approach 47
risk 47
complicated subsystem teams 10
composition pattern 147
configuration
externalizing 199
consistency 142
container image 89
containerization 21, 89, 222
containers
about 21
versus virtual machines 90
content-type versioning 175
context map
about 58, 59
relationships, between bounded context 59, 60
continuous delivery 21
continuous deployment (CD) 102
continuous integration (CI) 102
continuous integration/continuous delivery (CI/CD) 56
contract 243
contract testing 244
core microservices
versus API microservices 106
core priorities
defining, for business 19-21
correlation token
assigning, to service request 261
CQRS pattern
implementing, in microservices 141
CQRS, types
about 134
event sourcing 136, 137
single database 134
two-databases 135
Create, Read, Update, and Delete (CRUD) methods 73, 99, 187
cross-cutting
pitfalls 188-196
custom header versioning 175
