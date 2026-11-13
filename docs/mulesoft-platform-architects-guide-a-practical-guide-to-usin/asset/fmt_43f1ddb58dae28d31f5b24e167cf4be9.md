
- `Accept HTTP`: Allow incoming requests on the default SSL endpoint using the HTTP protocol
- `Drop HTTP`: Drop the HTTP request silently and accept only traffic on HTTPS

Figure 10.14 shows how to configure the HTTP requests, response timeout, and log downloads for the ingress load balancer in the private space:

![Figure 10.14 - Configuring HTTP requests for a private space](image/B19922_10__14.jpg)

Figure 10.14 - Configuring HTTP requests for a private space

## VPN connection

To connect a private space to the external network, a VPN connection can be established between them that includes an on-premises data center. It will create a secure and highly available connection between the private space and on-premises data center:

![Figure 10.15 - VPN IPSec tunnel for a private space](image/B19922_10__15.jpg)

Figure 10.15 - VPN IPSec tunnel for a private space

### VPN high availability

A private space allows you to create a second VPN connection if you have created only one VPN connection, and this is known as a redundant VPN. It is strongly recommended to create a redundant VPN as it will ensure high availability and will inherit some of the settings from the initial VPN, such as the routing type, `ASN`, and so on.

It is recommended to have two VPN connections to the customer gateway in case of any updates on one VPN connection or any failures. In such cases, the other VPN connection will ensure connectivity between the Anypoint Platform and the on-premises data center:

![Figure 10.16 - VPN high availability](image/B19922_10__16.jpg)

Figure 10.16 - VPN high availability

In the preceding architecture, the VPN is configured in such a way that it will be used in the following order in the event of any failures:

1. `VPN-1 Tunnel-1`
2. `VPN-1 Tunnel-2`
3. `VPN-2 Tunnel-1`
4. `VPN-2 Tunnel-2`

This is configured as an automatic failover to the other VPN tunnel or connection in the event of any failures. This VPN solution will be resilient and robust. Dynamic routing can help in advertising the route from `VPN-1` to `VPN-2` automatically, and it also supports static routing where `VPN-2` will be on standby mode in the event that `VPN-1` fails.

## Transit gateways

`AWS Transit Gateway` is like a router that helps you to simplify the network configuration and allows you to connect private spaces in AWS or Anypoint Platform, establishing the VPN connection toward on-premises or the Azure cloud or any third-party private network in a secure manner.

`CloudHub 2.0` allows you to attach multiple transit gateways in your organization, depending on the number of transit gateway license entitlements for your organization:

![Figure 10.17 - Transit Gateway attachments](image/B19922_10__17.jpg)

Figure 10.17 - Transit Gateway attachments

In Figure 10.17, we are connecting the private space to the on-premises data center via `AWS Transit Gateway` by creating a transit gateway attachment in Anypoint Platform.

## Private space network architecture

The following figure shows how applications deployed in the private space can communicate with the services or applications in the external or local network or over the internet. As we have discussed, local services can communicate with applications deployed in the private space via private endpoints, and external clients can communicate using public endpoints. You can configure public and private endpoints while deploying the application to the private space:

![Figure 10.18 - Private space network architecture](image/B19922_10__18.jpg)

Figure 10.18 - Private space network architecture

If you need to have a custom domain, you need to create a `TLS context` in the private space and add a domain certificate in `JKS` or `PEM` format. In the following figure, we have custom private (`internal-api.domainname.com`) and public (`api.domainname.com`) domains. To enable traffic to reach the application deployed in the private space over the custom domains, create a `CNAME` record in the vanity domain pointing to your private space **fully qualified domain name** (`FQDN`):

| `Custom domain (CNAME)` | `FQDN (A record)` | `Domain type` |
| --- | --- | --- |
| `internal-api.domainname.com` | `internal-<shard>.usa-e2.cloudhub.io` | Private |
| `api.domainname.com` | `<shard>.usa-e2.cloudhub.io` | Public |

Table 10.5 - `CNAME` ⇔ `A record` mapping

The private space will validate incoming requests against the inbound firewall rules, and all outgoing requests going from the private space will be validated by outbound firewall rules.

## Multiple environments in private spaces

Private space allows you to map a sandbox or production environment in the business group and its child group:

![Figure 10.19 - Mapping multiple environments for a private space](image/B19922_10__19.jpg)

Figure 10.19 - Mapping multiple environments for a private space

If you want to map the production and sandbox environments with the same private space, you can choose `Any environment`. However, it is recommended to have a separate private space for the sandbox and production environment.

## Multiple domains in private spaces

Private space allows you to create multiple `TLS contexts` to support multiple domains. For example, the sandbox is mapped to one private space having two environments: development and QA.

In such cases, we can configure two `TLS contexts` with the domain `api-dev.domainname.com` for the development environment and `api-qa.domainname.com` for the uat environment, as shown in Table 10.6.

| `CNAME` | `Private space – public domain target (A record)` | `Environment` | `Domain type` |
| --- | --- | --- | --- |
| `api-dev.domainname.com` | `<shard>.usa-e2.cloudhub.io` | Development | Public |
| `api-qa.domainname.com` | `<shard>.usa-e2.cloudhub.io` | QA | Public |

Table 10.6 - `CNAME` ⇔ `A record` mapping for multiple environments

![Figure 10.20 - TLS context for a private space](image/B19922_10__20.jpg)

Figure 10.20 - TLS context for a private space

We have three `TLS contexts`. One is the default `TLS context` that was created automatically while creating the private space. Then, we have two more `TLS contexts` for the development and QA environments.

Now, whenever we deploy an application to the development environment, we can select the host (`https://api-dev.domainname.com`) under the `Ingress` tab that is related to development, and whenever we deploy an application to the QA environment, we can select the host (`https://api-qa.domainname.com`) under the `Ingress` tab that is related to QA:

![Figure 10.21 - Application endpoints for a private space](image/B19922_10__21.jpg)

Figure 10.21 - Application endpoints for a private space

## Different options for deploying MuleSoft applications to CloudHub Runtime Manager

MuleSoft provides multiple ways of deploying applications to CloudHub, and all options can be used to integrate with the CI/CD pipeline for the automatic deployment of MuleSoft applications to CloudHub. Let’s explore all the options that can be used to deploy applications to CloudHub.

### Mule Maven plugin

The `Mule Maven plugin` is one of the important Maven plugins for deploying, un-deploying, and packaging MuleSoft applications into `CloudHub 2.0`. Let’s discuss the `Mule Maven plugin` in more detail. The following plugin can be configured in the `POM.xml` file of your application with all the required attributes that are needed for deploying and running the application in CloudHub:
