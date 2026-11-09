
it’s also possible for the entire process to stretch to 3 months or longer. Oh, and just a heads-up! If you’re aiming to drop your app during a peak time, such as around *Dreamforce* (you know—that massive annual Salesforce event), there might be some extra submission deadlines you’ll want to keep an eye on to get your results in time.

![](img/44ed632038957e4c80220676e84c5109_292_0.png)

Figure 6.1: Security review timeline

In terms of fees, for freemium, paid, or solutions requiring a paid add-on, the security review fee is $999 for each of the following scenarios: initial security review, resubmission after addressing detected issues from a previous security review, periodic re-review of an earlier approved solution, and when a partner requests an audit of a new version of their solution. You can submit up to four packages with the same namespace in a single submission, with a single payment of $999 per attempt.

Given that submissions typically require two attempts to pass, the average total cost for getting your application through the security review process is $1,998.

However, it’s important to note that security review fees are waived for free solutions. This means that whether it’s an initial security review, a resubmission after addressing detected issues in a previous security review, listing a new version of a solution that was previously approved, or a periodic re-review of a solution that was previously approved, there is no charge.

The fees may be subject to change, and the figures given are current as of 2023. You can check the most recent fees at https://developer.salesforce.com/docs/atlas.en-us.packagingGuide.meta/packagingGuide/security_review_fees.htm.

## Testing your application

Ensuring the security of your application is paramount before submitting it for the Salesforce security review. This process involves not just a review of your application’s architecture and components, but also its potential vulnerabilities. In the following subsections, you’ll learn some key practices to guide you in testing your application.

### Pre-assessment

Before submitting your application for a security review, perform an internal assessment of your solution to confirm its architecture is secure. Evaluate all aspects, such as object permissions, included components, custom code, web services, and third-party services being used. The aim is to secure all possible entry points and vulnerabilities that could be exploited to access data.

Your application should be robust, adhering to Salesforce best practices such as bulkifying your Apex code when dealing with records in triggers and avoiding code injection in **Salesforce Object Query Language (SOQL)**, Apex, or cross-scripting.

### Adversarial testing

After you’ve addressed all known security issues, try to find more! Encourage your testing team to think like hackers and attack your product to try to steal its data. This adversarial testing approach, coordinated with your team’s security advocate, could reveal vulnerabilities that have been overlooked. The *OWASP Web Security Testing Guide* is an excellent resource to help you devise a plan for adversarial testing. You can find it on the **Open Worldwide Application Security Project (OWASP)** web page: https://owasp.org/www-project-web-security-testing-guide/.

By following these strategies, you can submit your application for the security review with confidence, knowing you’ve done everything possible to secure your application. Your meticulous attention to potential vulnerabilities will not only ensure a smoother security review process but will also lead to a safer, more reliable product for your users.

## Components required for the AppExchange security review process

Running a successful security review process on AppExchange is like preparing for a big city marathon—it requires the right gear, a solid understanding of the race route, and, of course, the tenacity to see it through. This chapter is your personal fitness trainer, mapping out the must-have components for this marathon while also explaining why you need to tie your laces tight, wear breathable fabric, and carry energy bars. Or, in AppExchange terms, why you need a reliable test environment, well-organized documentation, effective static code analysis results, and clear false positive documentation.

### Preparing the test environment with an installed and configured managed package

Your test environment is like your running shoes in this marathon. Just as a seasoned runner needs the right shoes that provide support and prevent injuries, a good test environment forms a solid base for your application, helping identify any bugs or vulnerabilities.

![](img/44ed632038957e4c80220676e84c5109_295_0.png)

A well-prepared test environment is essential for the security review process. Use the Environment Hub, described in *Chapter 2*, to create a test environment, and then install your managed package and configure it. This environment should be a fully configured and authenticated demo org, complete with sample data to help the reviewer understand how your app works. Having a proper test environment ensures that the reviewer can effectively assess the security and functionality of your app.

Salesforce’s Environment Hub offers a streamlined solution for creating and configuring a new Salesforce org, which you can utilize as a security review test environment.

You can create a preconfigured security review test org using Salesforce Trialforce template ID 0TT3t000002nvUr¹. After setting it up, install and configure the latest version of the package in the test org.

1 https://salesforce.quip.com/pfqeA8kRTraY

Alternatively, you can utilize the Environment Hub to create an empty org and configure it manually. For seamless access and operations, it’s crucial to safelist Salesforce’s IP ranges. While you have the option to set these login ranges at the profile level, doing so at the network level can restrict the size of the IP range you’re allowed to set. To ensure your org’s security and accessibility, please safelist the Salesforce IP ranges provided in its official documentation². Additionally, to let Salesforce employees log in to your org seamlessly, include the following specific IPs and ranges:

2 https://salesforce.quip.com/XaCHAb5Yo15Y

- 13.108.0.0 - 13.111.255.255
- 62.17.146.0 - 62.17.146.255
- 85.222.134.0 - 85.222.134.255
- 104.161.246.0 - 104.161.246.255
- 122.15.249.154, 59.144.126.115
- 182.71.125.154 - 182.71.125.155
- 182.72.29.238 and 136.232.119.246

If **two-factor authentication (2FA)** is enabled in your test org, you should additionally disable it for the review process.

Remember—the package you install must match the version you’re submitting for the security review and should not be in a beta state. If an extra org setup is needed to use your app, make sure to do that as well.

If your test org is not configured correctly, the security review Operations team is going to notify you about it via email and specify the next steps:

Dear Partner,

Thank you for your recent Security Review submission for Formula Debugger. Before we can begin the review however, there are some issues with your submission which must be resolved:

### Regarding your submission:

- **Identity challenge is enabled on your org.**
  - you'll need to safe-list Salesforce IP ranges. you can set login ranges at the profile level (or you can also do it at the network level, but it limits the size of the IP range you can set.
  - Please safe-list the following Salesforce IPs ranges (https://help.salesforce.com/articleView?id=000321501&language=en_US&type=1&mode=1) and also the following:
  - 182.72.29.238, 182.71.125.154,
    182.71.125.155, 136.232.119.246, 13.110.78.8
  - ranges: 1>13.108.0.0 - 13.111.255.255
    2> 62.17.146.0 - 62.17.146.255
    3>85.222.134.0 -85.222.134.255
