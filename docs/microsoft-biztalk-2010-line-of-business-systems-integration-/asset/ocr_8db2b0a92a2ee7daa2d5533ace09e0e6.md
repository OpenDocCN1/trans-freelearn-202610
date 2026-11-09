# Understanding the ASDK-based adapter

If you read the product documentation, you will find that the ASDK-based adapters are built on top of the WCF Channel model and surface as custom WCF bindings. What this means is that WCF clients are able to communicate with ASDK-based adapters as if they were WCF services. Likely, the very first question you want to ask is whether the ASDK-based adapters are in fact WCF services just presented under the new fancy name. No, they are not! The use of the acronym WCF in the WCF LOB Adapter SDK is somewhat misleading; WCF forms the basis of the technology but the software does not revolve around web services. To understand how the adapters fit into the WCF infrastructure, let's recall some of the WCF fundamentals.

In order to establish communication process with clients, any WCF service must expose at least one endpoint. The WCF endpoints are based on three elements, known as the "A, B, and C" of the WCF. These three elements are:

+   - **Address:** It takes a form of the URI specifying the address where the service can be reached at.
+   - **Binding:** Bindings specify a communication protocol between the client and the service.
+   - **Contract:** Contracts specify what operations are exposed by the service.

Communication between client and service is conducted through communication channels; one channel on the client side and its equivalent on the server side. On the server side, when you instantiate the `ServiceHost` class, it instantiates a channel listener, which in turn builds a communication channel for the service. On the client side, the proxy creates a channel factory, which is responsible for building the channel for the client. The channels consist of binding elements, each responsible for its own part of message processing to form a stack of binding elements.