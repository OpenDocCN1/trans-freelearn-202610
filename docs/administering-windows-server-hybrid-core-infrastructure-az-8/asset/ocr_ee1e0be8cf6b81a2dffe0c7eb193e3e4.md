The protocols used for this VPN type are as follows:

+   - OpenVPN
+   - **Secure Socket Tunneling Protocol (SSTP)**
+   - IKEv2

This Point-to-Site VPN type is represented in the following diagram:

![](bbox=[0.25, 0.255, 0.73, 0.415])

Figure 6.6 – Point-to-Site VPN

The use case for a Point-to-Site VPN is when remote workers, such as home workers or those traveling, need access to Azure resources accessible via VNets and cannot use a Site-to-Site VPN.

# Site-to-Site VPN

A Site-to-Site VPN type provides an encrypted connection from on-premises locations to Azure resources by creating a secure tunnel across the internet for communication to support hybrid connectivity/cross-premises scenarios.

The Azure VPN gateway supports policy-based and route-based VPNs and the IPsec v1/v2 protocols; pre-shared keys are used for authentication.

This Site-to-Site VPN type is represented in the following diagram:

![](bbox=[0.25, 0.715, 0.73, 0.87])

Figure 6.7 – Site-to-Site VPN