# Finding 500 errors and XSS attacks in Jenkins through fuzzing

This recipe describes using a fuzzer to find server-side errors and XSS attacks in your Jenkins servers.

A fuzzer goes through a series of URLs, appends different parameters blindly, and checks the server's response. The inputted parameters are variations on scripting commands, such as `<script>alert("random string");</script>`. An attack vector is found if the server's response includes the unescaped version of the script.

Cross-site scripting attacks are currently one of the more popular forms of attack (http://en.wikipedia.org/wiki/Cross-site_scripting). The attack involves injecting script fragments into the client's browser so that the script runs as if it comes from a trusted website. For example, once you have logged in to an application, it is probable that your session ID is stored in a cookie. The injected script might read the value in the cookie and then send the information to another server ready for an attempt at reuse.

A fuzzer discovers the links on the site it is attacking and the form variables that exist within the site's web pages. For the web pages discovered, it repeatedly sends input based on historic attacks and lots of minor variations. If responses are returned with the same random strings sent, the fuzzer knows it has found an evil URL.

To fully integrate with the build process of a web-based application, you will need to build the application, deploy and run the application, run the fuzzer from a script, and finally, use log parsing to fail the build if evil URLs are mentioned in the output. This process will be similar for other command-line tools you wish to integrate. For more information about log parsing, refer to the *Deliberately failing builds through log parsing* recipe in Chapter 1, Getting Started with Jenkins.