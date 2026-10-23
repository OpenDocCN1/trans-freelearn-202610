![](bbox=[0.238, 0.124, 0.748, 0.395])

Figure 6.4 – Cloudflare EDR and endpoint device checks

Cloudflare can, for instance, use Azure AD as the IDP for getting user context and Carbon Black for collecting device context. Allowing it to integrate with many different providers makes it quite flexible in terms of device and identity context.

Now, unlike Microsoft and Citrix, Cloudflare does not have any VDI service, but does provide access to internal services, TCP/UDP, SSH/RDP, and web-based services using Warp and a server component called **Cloudflared**. cloudflared is the name of the service installed on a virtual machine running in our infrastructure. From an end user perspective, the Warp agent connects to Cloudflare networks, as seen in *Figure 6.5*. Then, the Cloudflare daemon runs on internal servers, either on Windows, Linux, or as a container, as seen in the following figure:

![](bbox=[0.134, 0.646, 0.849, 0.84])

Figure 6.5 – Cloudflare zero-trust overview