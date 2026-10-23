Cache behaviors can be configured with various options:

+   - **Path patterns**: Define which requests the cache behavior applies to, such as `/images/*` for all files in an image's directory or `/videos/high-def.mp4` for a specific video file
+   - **Viewer requests**: Based on factors such as request headers, cookies, query strings, and HTTP methods, determine whether to forward, redirect, or block user requests
+   - **Caching methods**: Set rules for how long content stays in the cache (TTL settings) and whether to cache based on headers, cookies, or query strings
+   - **Content handling**: Decide how CloudFront should handle content when it's not cached (e.g., fetch from the origin, serve a custom error page)

The benefits of using a cache include increased performance through optimized load times based on content type, fine-grained control over what content gets cached, cost-effectiveness by reducing the origin fetches, and enhanced security.

The next section will teach you how to secure and restrict content access.

# CloudFront Secure Access and Content Security

Accessing content via CloudFront is recommended for several reasons. In addition to the caching services and performance improvements offered by CloudFront, you can secure your content and how it is accessed using various security measures natively configurable within CloudFront. CloudFront integrates with other AWS security tools, such as WAF. You must understand the following critical areas when configuring access to your resources and implementing content restrictions.

In the following sub-section, you will learn how to restrict access based on geographical location.

# *CloudFront Geo Restriction*

Geo restriction, also known as geo-blocking, prevents users in specific geographic locations from accessing content you're distributing through CloudFront. If you have content rights that are only valid in certain countries, you can configure CloudFront to block or allow access based on the country from which the request originates. This concept is illustrated in Figure 9.3.