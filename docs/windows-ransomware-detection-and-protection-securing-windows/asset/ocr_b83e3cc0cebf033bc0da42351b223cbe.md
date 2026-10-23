# Network pillar – zero-trust maturity

As organizations are looking toward zero-trust-based networks, they need to align network access and security services according to the needs of their application/services instead of the implicit trust in traditional network security.

The network pillar shows the different functions of zero trust and shows the different levels of maturity that different organizations should strive to achieve:

| Function | Traditional | Optimized |
|---|---|---|
| Network segmentation | Network architecture using a large perimeter and macro segmentation, such as VLANs and zoning | Network architecture based on distributed ingress/egress micro-perimeters and internal micro-segmentation based on application and services |
| Threat protection | Threat protection primarily using known threats and static traffic filtering | Network-integrated machine learning with threat protection and filtering using context-based signals from other pillars |
| Encryption | Services are explicitly encrypted but minimal for internal and external traffic | All services and traffic are encrypted where possible |
| Automation and orchestration | Changes are implemented manually | Changes are implemented using orchestration tools such as CI/CD and IaC |
| Visibility and analytics | Visibility based on traffic patterns at the perimeter and centralized log analysis | Integrated analyses across multiple sensor types with automated alerts and triggers |

Table 2.3 – Network pillar of the zero-trust architecture

One of the most important aspects related to the network pillar is ensuring that segmentation is based on applications and their workloads instead of the more traditional zone-based approach. For instance, if a user needs access to a specific application that requires some form of VPN, the service will only provide a per-app-based VPN, and that access is around the application and not giving access to the entire network.

# Application pillar – zero-trust maturity

To ensure that applications are based on zero-trust principles, organizations need to ensure that they have protection mechanisms that provide visibility and vulnerability assessments into their applications