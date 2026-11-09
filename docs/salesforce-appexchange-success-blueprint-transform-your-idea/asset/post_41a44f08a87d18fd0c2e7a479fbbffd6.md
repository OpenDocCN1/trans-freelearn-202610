
| Factor |  |  |
|---|---|---|
| Interactivity and Integration Depth | Easy to implement interactive features and deeper integration, offering a richer user experience. | Implementing interactivity and deeper integration can be challenging due to the Canvas SDK’s proprietary nature. |
| Standard Compliance | Uses well-supported and widely understood internet standards, simplifying the development process. | Relies on proprietary SDKs and technologies that may not be universally understood, posing potential learning curves and integration challenges. |
| Backend and Frontend Separation | Maintains a clear separation between the backend and frontend, promoting cleaner, more maintainable code. | Often results in a mix of backend and browser scripts, complicating code management and the separation of concerns. |
| **Requirement of Connected App** | No need for a connected app, simplifying setup and reducing administrative overhead. | Requires a connected app, which adds complexity in packaging and setup. |
| **Support for Lightning and SLDS** | Fully supports Salesforce Lightning and Salesforce Lightning Design System (SLDS), ensuring a consistent UI and UX with the Salesforce environment. | Limited support for Lightning and SLDS, potentially leading to an inconsistent UI and UX compared to the standard Salesforce environment. |
| Starting and Scaling | Allows to start with a simple app and gradually scale up by adding features as per requirement. | The initial setup can be complicated, and scaling up by adding features might require a significant rework. |

For developing a lightweight application, the REST API integration appears to offer more advantages over a Canvas app, particularly in terms of standard compliance, ease of setup, and support for Lightning and SLDS.

Furthermore, the REST API’s support for iterative development makes it a more flexible choice for evolving application requirements.

## Summary

![](img/44ed632038957e4c80220676e84c5109_285_0.png)

Well done, valiant explorers! You’ve successfully completed this leg of our adventure, diving deep into the world of seamless integration with external systems in Salesforce. Your valiant efforts to decipher the enigma of technical architecture and your dedication to nurturing various integration patterns, just like a dutiful gardener, have rewarded you with bountiful knowledge.

Your brave exploration led you through the realm of Salesforce APIs, akin to unearthing hidden treasures. You’ve uncovered the magic of Salesforce Connect, your personal teleportation device, ready to whisk data from distant lands into your Salesforce kingdom. The Canvas SDK revealed itself as your loyal craftsman, embedding third-party applications seamlessly into Salesforce, and connected apps stood as vigilant gatekeepers, ensuring safe passage between Salesforce and external systems.

As we bid adieu to this chapter, remember the insights you’ve gathered are invaluable in building robust applications that integrate effortlessly with external systems. But hold on to your explorer’s hat, because our next adventure beckons us into the security review process. We’ll decipher the cryptic scrolls of Apex and LWC code requirements, and learn how to survey external systems and endpoints for potential threats. You’ll master the tools of the trade, Salesforce Code Analyzer and Checkmarx, to ensure your code’s security and compliance. We’ll also prepare documentation and identify false positives, essential to successfully navigating the security review. So, tighten your grip on the adventurer’s compass as we continue our thrilling journey toward AppExchange success!

## Security Review

![](img/44ed632038957e4c80220676e84c5109_287_0.png)

Greetings, bold adventurers of the digital realm! As we traverse the ever-evolving landscape of Salesforce, we face a crucial challenge—the fierce dragon known as “security review.” Aye, ’tis a formidable beast, but fear not! Our noble quest, armed with the right knowledge and tools, is to tame this dragon, win its trust, and ultimately ensure the security of our precious treasure—customer data.

Prepare for a journey through the following territories:

- The significance of the security review process and its role in maintaining high-security standards in Salesforce applications
- Ensuring compliance with Salesforce security requirements and avoiding common pitfalls
- Tools and techniques for scanning external systems and endpoints
- Preparing false positive documentation for a smoother security review process
- Embracing the journey of continuous security improvement as an AppExchange partner

By the end of this chapter, you will be a seasoned navigator of the security review process, equipped to build trust with your Salesforce subjects and ensure the flourishing of your application within the grand kingdom of AppExchange.

Tighten your boots, fellow explorers! Our journey continues, driven by the famous words of Salesforce CTO, Parker Harris, “Nothing is more important to our company than the privacy of our customers’ data.” Thus we march, toward a future of secure Salesforce adventures!

## Understanding the security review process

![](img/44ed632038957e4c80220676e84c5109_289_0.png)

Welcome to the exhilarating marathon of developing your first AppExchange application! You’re tying up your laces at the starting line of a course where the sprint of innovation links arms with the endurance of security. Salesforce, the world-class race organizer in the marathon of **customer relationship management** (**CRM**) platforms, has staked its reputation on one thing above all—trust. And a significant water station on this trust marathon? The security review process—an exhaustive vetting that each application must clear, akin to our marathon’s rigorous checkpoints, before making it to the finish line at the Salesforce AppExchange marketplace.

Think of the security review process as your personal marathon trainer, diving into the granularities of your application’s design, architecture, and data handling practices. Its mission? To uncover those sneaky, potential security cramps and ensure your application is fit enough to run Salesforce’s marathon of high-security standards. It’s the thorough pat-down at the starting line, scanning everything from your application’s running gear (source code) and energy gels (data storage) to its hydration strategy (transmission methods).

As committed marathoners in this AppExchange race, understanding the security review process and its race rules is not a “nice-to-have”—it’s your race-day strategy. It’s not just a daunting hill to conquer but a pacing guide, helping you avoid those brutal wall-hitting moments. Its job? To help you spot potential security side-stitches early enough in the training cycle, giving you ample time to tweak your stride, hydrate, or whatever it takes to keep those issues from slowing you down. This strategic approach to security is not only your ticket to a safe and successful race day (read: application) but also your personal PR in the trust-building race with your users.

Training with security at the forefront is like diligently following a well-planned marathon training program—it’s tough and requires dedication, but boy does it pay off! It steers you clear from those nasty, time-guzzling injuries (read: revisions) later in the race and boosts your odds of breezing through the security review checkpoint at the very first attempt. Plus, it shows your users that their security isn’t an eleventh-hour, carb-loading attempt for you, but a well-thought-out part of your training regimen. And who knows, this could be the secret sauce that nudges them to cheer for your application on the big day!

## Fees and timeline

The security review process involves several key steps, as follows:

- 1. You submit your application for security review via the Partner Community.
- 2. The security review Operations team verifies your submission, a process that typically takes 1-2 days.
- 3. Your submission is then added to the Product Security queue.
- 4. The Product Security team performs tests on your application and validates the results.
- 5. Finally, the security review Operations team notifies you of the results.

The timeline for *steps* 3-5 typically spans 4-6 weeks. However, it’s worth noting that most ISVs don’t pass the security review on their first attempt. While some applications might get approved on their first or second try, others might need a third attempt or even more. As a result, you can optimistically anticipate your application to be live on the AppExchange marketplace within a minimum of 4 weeks from your initial submission. Yet,
