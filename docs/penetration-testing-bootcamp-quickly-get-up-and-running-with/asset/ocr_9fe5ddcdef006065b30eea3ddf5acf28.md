# 8

# Web Application Attacks

Web applications provide some of the greatest attack surfaces for organizations. With so many web applications that are publicly available for providing services, one must make sure they are secure. With millions of users utilizing these applications daily, and inputting all sorts of data into them, like personally identifiable information, credit card information, health information just to name a few, having a web application compromised leaves all that data available for the hacker.

With that being said, there are a lot of moving pieces to web applications, so running a penetration test against them can be daunting. I have laid out a couple of different sections that I test when running a penetration test against web applications and included various tools to test within that category. These categories follow OWASP recommendations as some of the important pieces to verify during a penetration test. In case you are not familiar with OWASP, or the open web application security project, you can get more information here: https://www.owasp.org/index.php/Main_Page. In the simplest form, they are a nonprofit organization with the sole mission to make software security visible to all, so that individuals can understand them, and therefore, make better security decisions around web applications. OWASP does publish a top 10 every couple of years to really show application developers the current trends and threats to web applications. The last year published was 2013, and this year, 2017, a new list will be published later on in the year. The proposed list for 2017 includes the following:

+   - Injection
+   - Broken authentication and session management
+   - XSS
+   - Broken access control
+   - Security misconfiguration
+   - Sensitive data exposure
+   - Insufficient attack protection