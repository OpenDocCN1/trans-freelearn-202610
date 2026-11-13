
As shown in Figure 18.10, we have configured Anypoint VPC, DLB, and VPN. The DLB will allow the hosting of all the APIs under a single domain and Anypoint VPN will allow connection to the resources located in the corporate data center.

J&J Music can enable API management capabilities for the APIs running on CloudHub 1.0 as shown in Figure 18.11.

![Figure 18.11: CloudHub 1.0 with API management](image/B19922_18_11.jpg)

As shown in Figure 18.11, the API manager can be peered with APIs deployed to CloudHub 1.0 using API Autodiscovery. This will allow us to manage and secure the APIs.

## Use case 2

Java is used to write J&J Music’s APIs, which are hosted on-premises. To protect, manage, discover, and engage with the APIs, they are currently trying to activate API management features on the APIs. Additionally, they intend to gradually move every API to the cloud. MuleSoft is already the platform of choice for them to manage and implement the APIs.

From the use case, J&J Music wants to use Anypoint Platform to manage non-MuleSoft APIs. They also need to gradually migrate everything to CloudHub.

There are multiple solutions that can be used to implement this use case.

Hybrid implementation is an option (the control plane in Anypoint Platform and the runtime plane in the customer data center).

We must set up an API proxy that will peer with Java APIs to enable API management to manage Java APIs. Although the on-premises Mule runtime will enable this API proxy to operate closer to customer Java APIs, it does not fully satisfy the client’s requirements as the client intends to migrate to the cloud soon. According to J&J Music’s plan to migrate to the cloud, this solution will not be practical in the long run.

To fully meet J&J Music’s requirements, we have selected CloudHub 2.0 as our hosting solution. To connect the Java APIs running on-premises via an API proxy running on CloudHub 2.0, we will also need to configure the private space on CloudHub 2.0 and the Anypoint VPN. More information regarding CloudHub 2.0 was covered in [Chapter 10](#B19922_10.xhtml#_idTextAnchor228). This option will be more adaptable and fit the needs and long-term goals of the client.

![Figure 18.12: CloudHub 2.0 with API proxy](image/B19922_18_12.jpg)

Figure 18.12 shows what J&J Music’s architecture will look like for managing the Java APIs using Anypoint Platform.

> **Note**
>
> Like CloudHub 2.0, CloudHub 1.0 can also be utilized in a similar manner.

Flex Gateway can be used to manage Java APIs; [Chapter 15](#B19922_15.xhtml#_idTextAnchor405) has more information on Flex Gateway. AWS, Azure, Google, and other cloud platforms can be used to set up Flex Gateway.

## Use case 3

J&J Music desires to expand its business in a country with stricter laws and compliance requirements. The government of that country made cybersecurity, data protection, and privacy its top priorities through several laws and regulations. The rules of the country state that if you want to do business online, you must host your infrastructure there or use a cloud provider that gives you the option to host infrastructure there. Additionally, your data and metadata are not allowed to leave the country.

Although MuleSoft’s control plane is situated in the US or the EU, it is not accessible in the country where we wish to grow our business and make our online presence possible. We are unable to manage the API life cycle in this scenario using Anypoint Platform. We must put into practice Anypoint PCE, which can be hosted in that country in a cloud computing environment or in the corporate data center.

Customer-hosted Anypoint PCE will be used to manage the APIs, and customer-hosted Mule runtime will be used to execute Mule applications.

![Figure 18.13: Anypoint PCE and customer-hosted Mule runtime](image/B19922_18_13.jpg)

Figure 18.13 shows that the runtime plane and control plane are hosted in customer infrastructure. This will allow J&J Music to be compliant with the laws and regulations defined by the country.

> **Note**
>
> On-premises Mule runtime can be implemented instead of RTF.
>
> If the cloud provider permits hosting the necessary infrastructure in that country, the control plane and runtime plane may be hosted there.

## Summary

In this chapter, we talked about the main components provided by MuleSoft. We have also seen flexible API techniques in the design and development of resilient APIs. `Anypoint CLI` for managing the MuleSoft platform and APIs was also discussed, including how `Anypoint CLI` can be integrated with `CI/CD` for application deployment and automation. We also discussed things to consider when choosing the right deployment option to run a MuleSoft application.

Thanks for taking the time to read this book. Your attention to this means a lot to me and is very much appreciated. Thank you.

## Index

As this ebook edition doesn't have fixed pagination, the page numbers below are hyperlinked for reference only, based on the printed edition of this book.

A

accelerator [85](#B19922_05.xhtml#_idIndexMarker283)

accelerator building blocks [86](#B19922_05.xhtml#_idIndexMarker284)

best practices [88](#B19922_05.xhtml#_idIndexMarker299)

connectors [87](#B19922_05.xhtml#_idIndexMarker291)

data mappings [88](#B19922_05.xhtml#_idIndexMarker300), [89](#B19922_05.xhtml#_idIndexMarker303)

endpoints [89](#B19922_05.xhtml#_idIndexMarker305)

pre-built APIs [87](#B19922_05.xhtml#_idIndexMarker285)

templates [88](#B19922_05.xhtml#_idIndexMarker294)

Amazon Virtual Private Cloud (VPC) [198](#B19922_10.xhtml#_idIndexMarker708)

Amazon Web Services (AWS) [304](#B19922_13.xhtml#_idIndexMarker975)

Anypoint API Manager

monitoring and analytics capabilities [316](#B19922_14.xhtml#_idIndexMarker1026), [317](#B19922_14.xhtml#_idIndexMarker1027)

Anypoint API Manager alerts [387](#B19922_15.xhtml#_idIndexMarker1209)

failed requests [387](#B19922_15.xhtml#_idIndexMarker1212)

policy violation [387](#B19922_15.xhtml#_idIndexMarker1213)

request volume [387](#B19922_15.xhtml#_idIndexMarker1211)

response time [387](#B19922_15.xhtml#_idIndexMarker1210)

Anypoint CLI [192](#B19922_09.xhtml#_idIndexMarker697), [193](#B19922_09.xhtml#_idIndexMarker699), [354](#B19922_15.xhtml#_idIndexMarker1111), [433](#B19922_18.xhtml#_idIndexMarker1346)

features [434](#B19922_18.xhtml#_idIndexMarker1348)

reference link [193](#B19922_09.xhtml#_idIndexMarker700)

Anypoint clustering [272](#B19922_12.xhtml#_idIndexMarker891)

benefits, for on-premises Mule runtime [273](#B19922_12.xhtml#_idIndexMarker895)

concurrency issues, solving [274](#B19922_12.xhtml#_idIndexMarker900)

configuration properties [274](#B19922_12.xhtml#_idIndexMarker903), [275](#B19922_12.xhtml#_idIndexMarker904)

multicast clustering [273](#B19922_12.xhtml#_idIndexMarker898)

persistent object store [277](#B19922_12.xhtml#_idIndexMarker911)

primary node and secondary node [278](#B19922_12.xhtml#_idIndexMarker913)\-[280](#B19922_12.xhtml#_idIndexMarker916)

Quorum Management [276](#B19922_12.xhtml#_idIndexMarker906)

setting up, manually [274](#B19922_12.xhtml#_idIndexMarker902)

setting up, on Anypoint Platform [276](#B19922_12.xhtml#_idIndexMarker910)

unicast clustering [273](#B19922_12.xhtml#_idIndexMarker896)

VM queues [281](#B19922_12.xhtml#_idIndexMarker918)

Anypoint Code Builder (ACB) [18](#B19922_01.xhtml#_idIndexMarker071), [46](#B19922_02.xhtml#_idIndexMarker187)

Anypoint Community Manager (ACM) [355](#B19922_15.xhtml#_idIndexMarker1115), [381](#B19922_15.xhtml#_idIndexMarker1187)

features [381](#B19922_15.xhtml#_idIndexMarker1188)

limitations [382](#B19922_15.xhtml#_idIndexMarker1190)

Anypoint control plane [31](#B19922_02.xhtml#_idIndexMarker139)

business groups [36](#B19922_02.xhtml#_idIndexMarker156), [37](#B19922_02.xhtml#_idIndexMarker157)
