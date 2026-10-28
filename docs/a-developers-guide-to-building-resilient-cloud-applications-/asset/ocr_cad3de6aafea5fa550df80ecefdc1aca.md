A certificate can include the following properties:

+   - **Certificate authority (CA)**: The API allows only certificates signed by a particular CA
+   - **Thumbprint**: The API allows only certificates including a specified thumbprint
+   - **Subject**: The API allows a specific subject (mentioned in the certificate)
+   - **Expiration date**: The API only allows certificates that have not expired and are still available

# Accepting client certificates in the consumption tier

If you build your APIs from serverless technologies, such as Azure Functions, the Consumption tier in API Management is designed to meet serverless design principles.

You must explicitly enable the use of client certificates for the Consumption tier, which you can do on the **Custom domains** page.

A custom domain is a domain or subdomain we can buy from a provider and use for personal or professional purposes (e.g., `yourdomain.com` or `pages.yourdomain.com`).

![](bbox=[0.29, 0.45, 0.72, 0.64])

In the previous screenshot, we can see that we are able to add one or more client certificates. We can enable requesting a client certificate by selecting **Yes**. Next, we will create policies.

# Certificate authorization policies

There are the following different authorization policies:

+   - Inbound policies are executed when the API Management API is called
+   - Backend policies are executed when API Management calls the backend APIs
+   - Outbound policies are executed when API Management returns the response to the caller