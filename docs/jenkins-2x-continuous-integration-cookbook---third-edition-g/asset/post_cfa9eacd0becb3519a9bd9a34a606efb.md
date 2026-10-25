
## How it works...

Wapiti loads in different modules. By default, all modules are used. You will have to be selective; for Version 2.2.1 on Ubuntu Linux, this causes Wapiti to crash or timeout.

To load in specific modules, use the `-m` option.

The `-m "-all,xss,exec"` statement tells Wapiti to ignore all modules except the xss and exec modules.

The exec module is very good at finding 500 errors in Jenkins. This is mostly due to unexpected input, which Jenkins does not handle well. This is purely a cosmetic set of issues. However, if you start to see errors associated with resources such as files or database services, then you should give the issues higher priority and send in bug reports.

The `-x` option specifies which URLs to ignore. In this case, we don't want to cause work for the plugin manager. If we do, it will then generate a lot of requests to an innocent external service.

Wapiti crawls websites. If you are not careful, the tool might follow a link to locations that you do not want testing. To avoid embarrassment, carefully use the exclude URL's option, `-x`.

The `-v2` option sets the verbosity of logging up to its highest so that you can see all the attacks.

In the second run of Wapiti, you also used the permanentxss module, which at times finds bonafide XSS attacks, depending on the race between developers building features and cleaning bugs:

Fuzzers are good at covering a large portion of an application's URL space, triggering errors that would be costly in terms of time to search out. Consider automating through a Jenkins job as part of a project's QA process.

## There's more...

The reports you generated in this recipe mention many more server errors than XSS attacks. This is because many of the errors generated are due to unexpected input causing failures that are only caught by the final layer of error handling; in this case, the bug report page. If you consider the error worth reporting, then follow the instructions found on the bug report page.

Here are some guidelines for the meaning behind the output of the stack traces:

- `java.lang.SecurityException`: If a Jenkins user is doing something that the programmer considers insecure, such as hitting a URL, this should only be reachable once you have logged in.
- `java.lang.IllegalArgumentException`: Jenkins checked for a valid range for your parameter and the parameter value was outside that range. This is a deliberately thrown exception.
- `java.lang.NumberFormatException`: Jenkins did not check for a valid string and then tried to parse a non-conformant string to a number.
- `java.lang.NullPointerException`: This normally happens when you hit a URL without all the parameters set that Jenkins expects. In programmer's language, the code is expecting an object that does not exist, and then tries to call a method of the nonexistent object without checking that the object exists. The programmer needs to add more error-checking. Write a bug report.

## See also

- The *Testing for OWASP's top 10 security issues* recipe

## Avoiding sign-up bots with JCaptcha

**CAPTCHA** stands for **Completely Automated Public Turing test to tell Computers and Humans Apart**. The most commonly viewed CAPTCHAs are sequential letters and numbers displayed as graphics that you have to correctly feed into a text input.

If you let anyone sign up for an account on your Jenkins server, then the last thing you want are bots (automated scripts) creating accounts. Bots have an economy of scale, being able to scan the internet rapidly and never getting bored. CAPTCHAs are a necessary defense against these dumb attacks.

The negative purposes of bots are as follows:

- Performing a **Denial Of Service (DOS)** attack on your server, for example, by automatically creating numerous heavyweight jobs
- **Distributed Denial Of Service attack (DDOS)** on other servers by harvesting many Jenkins servers to fire off large numbers of requests
- Injecting unwanted advertisements or content that then points to malicious sites
- Adding scripts that are stored permanently and run when a user accidentally browses the Jenkins site

There are commercial motivations for criminals to circumvent CAPTCHAs that have led to well-documented law cases. You can find one such law case at [http://www.wired.com/2010/10/hacking-captcha/](http://www.wired.com/2010/10/hacking-captcha/).

## Getting ready

Make sure you have backed up your sacrificial Jenkins server. You are going to modify its security settings. It is easy to make a service-changing mistake.

The JCaptcha plugin is based on Java implementation that you can find at https://jcaptcha.atlassian.net/wiki/display/general/Home.

## How to do it...

- 1. Log in as an administrator.
- 2. Click on the Configure Global Security link.
- 3. Select Jenkins' own user database under Security Realm.
- 4. Select Allow users to sign up:
- 5. Press Save.
- 6. Browse to the signup location, http://localhost:8080/signup.
- 7. Click on the Manage Plugins link in the Manage Jenkins page.
- 8. Select the Available tab.
- 9. Install the JCaptcha plugin.
- 10. Click on the Configure Global Security link under the Manage Jenkins page.
- 11. Select Jenkins' own user database under Security Realm.
- 12. Select Enable captcha on sign up.
- 13. Press Save and then click on the Log Out link.
- 14. Browse to the signup location, http://localhost:8080/signup. The page is now defended by a CAPTCHA.

## How it works...

Installing the plugin adds the CAPTCHA image to the signup process. The image needs pattern recognition to decipher it. Humans are very good at this; automated processes are a lot worse, but improving.

## Improving Code Quality

In this chapter, we will cover the following recipes:

- Integrating Jenkins with SonarQube
- The updating center in SonarQube
- Quality gates, quality profiles, and rules
- Verifying HTML, CSS, and JavaScript validity using SonarQube
- Verifying Java code using SonarQube
- Configuring SonarQube as a Windows service

## Introduction

This chapter explores the use of Jenkins plugins to display code metrics and failed builds. Automation lowers costs and improves consistency. The process does not get tired. If you decide the success and failure criteria before a project starts, then this will remove a degree of subjective debate from release meetings.

In 2002, NIST estimated that software defects were costing America around 60 billion dollars per year (http://www.abeacha.com/NIST_press_release_bugs_cost.html). Expect the cost to have increased considerably since. To save money and improve quality, you need to remove defects as early in the software lifecycle as possible.

You will also find recipes in this chapter on static code review through SonarQube. Static means that you can look at the code without running it. Good documentation and source code structure aid the maintainability and readability of your code.

There are a number of good introductions to software metrics. These include a Wikibook on the details of the metrics (http://en.wikibooks.org/wiki/Introduction_to_Software_Engineering/Quality/Metrics) and a well-written book called *Code Quality: The Open Source Perspective*, by Diomidis Spinellis.

In the *Integrating Jenkins with SonarQube* recipe of this chapter, you will link Jenkins projects to Sonar reports. Sonar is a specialized tool that collects software metrics and breaks them down into understandable reports. Sonar details the quality of a project. It uses a wide range of metrics including the results of tools such as FindBugs and PMD, mentioned in this chapter. The project itself is evolving rapidly. Consider using Jenkins for early warnings and to spot obvious defects, such as a bad commit. You can then use Sonar for a deeper review.

Download SonarQube from https://www.sonarqube.org/downloads/ and extract it in the system:

![](img/2a4d2e09d9af2c678ea8f43ea344059d_274_0.png)

SonarQube supports 20+ programming languages: Java Android, JavaScript, HTML, CSS, Objective-C, C#, C/C++, PHP, Python, PL/SQL, Flex, Groovy, COBOL, Swift (paid plugin), and so on. It allows reports on bugs, vulnerabilities, code smells, duplicate code, code coverage, comments, and so on.

- 1. Java 8 is a prerequisite for running SonarQube.
- 2. In the Windows OS, go to the `bin` directory and, based on the operating system and platform of the operating system, go to the specific directory.
