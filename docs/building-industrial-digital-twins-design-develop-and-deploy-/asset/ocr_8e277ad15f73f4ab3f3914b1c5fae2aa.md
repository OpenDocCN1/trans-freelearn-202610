+   - Full deployment
+   - Value proposition and tracking

In the first part of this chapter, we will assess how the Digital Twin addresses the functional requirements initially set in Chapter 6, Building the Digital Twin Prototype.

# # Functional testing of the Digital Twin

Functional testing is standard practice in developing software applications, and creating Digital Twin solutions is no exception. For Digital Twins, however, we recommend functional testing of both the Digital Twin infrastructure and the Digital Twin application.

In Chapter 5, Setting Up a Digital Twin Prototype, we created models and instances of a wind farm and wind turbines as your Digital Twin prototype solution. Testing the functionality and integrity of the models and ensuring that they are correctly deployed is the first step of functional testing of a Digital Twin. Once we’ve confirmed that our Digital Twin infrastructure is working effectively, we can test the actual Digital Twin applications configured in Chapter 6, Building the Digital Twin Prototype.

Let’s look at the test framework next.

# # Testing the Digital Twin infrastructure

Your wind-farm Digital Twin prototype was configured on the ADT **platform as a service** (PaaS). It is one example of an **integration PaaS** (iPaaS) for Digital Twin infrastructure. We will continue with that example as we describe the first step in the functional testing of your Digital Twin prototype.

The purpose of the infrastructure functional test is to ensure that the platform components are correctly configured. This functional test will eliminate incorrect platform configuration as a potential reason for failing the actual application functional test.

We propose the testing sequence shown in the following diagram for the infrastructure functional test:

![](bbox=[0.2461, 0.7578, 0.7617, 0.8422])

Figure 7.1 – Functional testing Digital Twin infrastructure