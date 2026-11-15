

# Common Website Attack Types

| Attack Type | Description |
|-------------|-------------|
| **Security Misconfiguration** | This is one of the most common issues. Keeping `DEBUG = True`, exposing secret keys, or failing to restrict allowed hosts can leak sensitive information or open the door for attacks. |
| **File Upload Abuse** | If your app allows uploads, attackers may try to upload malicious files or oversized files to exhaust resources. |
| **Denial of Service (DoS)** | Instead of breaking in, attackers flood the site with traffic or expensive requests. For small apps, this is usually application-layer abuse, not a massive network flood. |

**Table 15.1: Common Website Attack Types**

---

# OWASP Top 10

The **Open Web Application Security Project (OWASP) Top 10** is a widely recognized list of the most common and impactful security risks affecting web applications. Maintained by a global community of security professionals, it highlights patterns seen in real-world breaches. It is not an exhaustive list, but a practical guide to the vulnerability types developers are most likely to encounter, helping teams prioritize what matters most when building and deploying secure apps.

The OWASP Top 10 consistently highlights broken access control, injection, and misconfiguration as leading causes of web app vulnerabilities: [https://owasp.org/Top10/2025/](https://owasp.org/Top10/2025/)

> **Tip**: Please provide examples of XSS, CSRF, and SQL Injection in a Django application.

---

# Practical Security Baseline for Beginners

You don't need expensive tools to secure a small Django app. A simple, well-configured setup goes a long way.

Start with Django's deployment checklist:

1.  Set `DEBUG = False`
2.  Define `ALLOWED_HOSTS`
3.  Keep `SECRET_KEY` private
4.  Run `python manage.py check --deploy`

Django explicitly warns that running with `DEBUG = True` in production exposes stack traces, settings, and sensitive internal details.

> **Tip**: What does Django's `check --deploy` command actually verify under the hood?

---

# Enforce HTTPS and Secure Cookies

Django recommends enforcing HTTPS across your entire site, not just the login page, because session cookies are shared across requests. Always serve your site over HTTPS. This protects login sessions and user data.

The following settings should be enabled in your Django settings:

```python
if not DEBUG:  # Enable only in production
    SECURE_SSL_REDIRECT = True
    SESSION_COOKIE_SECURE = True
    CSRF_COOKIE_SECURE = True
    SECURE_HSTS_SECONDS = 31536000  # 1 year
    SECURE_HSTS_INCLUDE_SUBDOMAINS = True
    X_FRAME_OPTIONS = "DENY"
    SECURE_CONTENT_TYPE_NOSNIFF = True
```

Notes on the above settings:

* `SECURE_SSL_REDIRECT = True`: Forces all HTTP requests to redirect to HTTPS. If a user visits `http://your-site.com`, Django automatically redirects them to `https://your-site.com`. This ensures all communication is encrypted. Without this, users might access your site over plain HTTP, allowing attackers to intercept or modify traffic (Man-in-the-Middle attacks).
* `SESSION_COOKIE_SECURE = True`: Ensures the session cookie is only sent over HTTPS. Browsers will not include the session cookie in HTTP requests. Session cookies identify logged-in users. If sent over HTTP, they can be intercepted and used to hijack the session.
* `CSRF_COOKIE_SECURE = True`: Ensures the CSRF cookie is only sent over HTTPS. Similar to the session cookie, but specifically for the CSRF token. CSRF protection relies on a secret token. If that token is exposed over HTTP, an attacker could bypass CSRF protection.
* `SECURE_HSTS_SECONDS = 31536000` (1 year, in seconds): Enables **HTTP Strict Transport Security (HSTS)** and tells browsers to always use HTTPS for your site for the next year. After visiting your site once over HTTPS, the browser will refuse to connect via HTTP for the specified duration. It prevents downgrade attacks where an attacker tries to force a user back to HTTP.
  
  > **Warning!** Only enable this once you are certain HTTPS works correctly everywhere.

* `SECURE_HSTS_INCLUDE_SUBDOMAINS = True`: Extends HSTS to all subdomains. E.g., `api.your-site.com`, `admin.your-site.com`, etc. Attackers often target weaker subdomains. This ensures the entire domain is HTTPS-only.
* `X_FRAME_OPTIONS = "DENY"`: Prevents your site from being embedded in an `<iframe>`. Browsers will block any attempt to load your pages in a frame on another site. Prevents clickjacking attacks, where a malicious site tricks users into clicking hidden elements on your site.
* `SECURE_CONTENT_TYPE_NOSNIFF = True`: This tells browsers not to guess (or "sniff") the content type of a response. Browsers must respect the `Content-Type` header sent by the server. Prevents certain attacks where a file is disguised as one type (e.g., text) but executed as another (e.g., JavaScript).

As a group, these settings enforce HTTPS everywhere, protect sensitive cookies, prevent common browser-based attacks (clickjacking, MIME sniffing), and reduce the risk of traffic interception and tampering.

They are part of Django's **secure-by-default for production** approach, but they only work correctly if your app is properly deployed behind HTTPS (e.g., using Nginx or a cloud platform that terminates TLS).

> **Warning!** Do not blindly enable these in development, because local environments usually don't have HTTPS configured, and the redirects and secure cookies will break your development workflow.

---

# Protect Against XSS and CSRF

Django helps here, but only if you follow its conventions:

* Use Django templates instead of manually building HTML strings
* Avoid marking user input as safe unless absolutely necessary
* Ensure all forms include the CSRF token, like this:

```html
<form method="post">
    {% csrf_token %}
    <!-- form fields -->
</form>
```

For extra protection, consider enabling **Content Security Policy (CSP)**, which restricts where scripts can be loaded from. Django 6 includes built-in support for CSP configuration, such as `SECURE_CSP` settings, but earlier versions like the current LTS 5.2 do not.

CSP is a browser-enforced security mechanism that controls which resources a page is allowed to load and execute. It is defined by the server (usually via an HTTP response header) and tells the browser things like: which domains JavaScript can be loaded from, whether inline scripts are allowed, where images, stylesheets, fonts, and other assets can come from.

The goal is to reduce the impact of attacks like **Cross-Site Scripting (XSS)**. Even if an attacker manages to inject malicious code into your page, a well-configured CSP can prevent that code from running because it doesn't come from an approved source.

A simple example might look like this:

```
Content-Security-Policy: default-src 'self'; script-src 'self' https://trusted.cdn.com;
```

The header above tells the browser: by default, only load resources from your own site, and only allow scripts from your site and a trusted **Content Delivery Network (CDN)**.

CSP works best as a defense-in-depth layer. It doesn't replace secure coding practices, but it makes exploitation much harder when things go wrong.

