```
if "ROLE_EDIT" in SecurityContextHolder.getContext().
                        authentication.granted_auths:
    footer += """
    <ul>
        <li><a href="/edit/""" + self.article + """>Edit
            </a></li>
    </ul>
    """

    footer += """
        <a href="http://springpythonbook.com">Spring Python book
        </a>
    </body>
    """
    return footer
```

SecurityContextHolder is a globally accessible object. It contains a context, which specifies where the current authentication credentials are being stored.

+   - If SecurityContextHolder is configured with mode "GLOBAL", then there is a single context for the entire Python VM, meaning all threads will see the same security credentials
+   - If SecurityContextHolder is configured with mode "THREADLOCAL", then the context is stored in threading.local(), meaning there is a separate context for each thread. This is useful for multi-threaded server apps where a separate thread exists for each user

By the time this code is actually executed, SecurityContextHolder will have been populated with user data, allowing us to do a quick check and only offer this link of the user has the necessary role, without altering our application API. This satisfies the requirement: 'credential data and other security APIs must be available non-intrusively'.

# Testing application security

Testing security has traditionally been a challenge, because there are many facets to cover. Integrating with a 3rd party authentication system is one part that needs to be checked out. Making sure the application is using the security system is also needed. And confirming that both good and bad accounts are handled properly is important. Often, we tend to code successful scenarios. Its important to know that our software responds to failing scenarios as well. While this is important for general coding, it is especially critical to test failing security scenarios. If the system doesn't properly handle invalid or expired credentials, what is the point in securing the system?