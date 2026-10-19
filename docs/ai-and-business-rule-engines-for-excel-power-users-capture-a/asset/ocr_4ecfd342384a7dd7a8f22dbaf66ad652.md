+   3. Click on >_. This will cause your application to restart; it will also open a panel at the bottom of the screen that looks like a terminal, but that’s a side effect and we don’t need to type anything into it.

All of these restrictions won’t cause an issue during development, but if you do intend to deploy your decision models for real-life business use, it is much better to migrate to the paid-for tier.

> **Remember the online resources**
> The KIE Sandbox and Red Hat teams have put a lot of effort into the online resources we mentioned in *Chapter 2*. Now is a good time to go look at those again. There is a particularly good video on linking KIE Sandbox with OpenShift at https://youtu.be/2zY85TXFNuw (or search the KIE YouTube channel for *DMN Dev Sandbox Developing and deploying DMN decisions in the cloud*).

Since we’ve made an important step into deploying our models outside of our own laptops, it’s probably a good time to have a conversation about how to treat data.

# Taking care of your data

We’re glad about the trend of legislation toward increased data privacy. As professionals who work with data, we have been known to curse the regulations sometimes, but in many ways, it’s just ensuring what we hope you would seek to do anyway: *be respectful when working with personal data.*

Personal data is anything that could identify a person—a phone number associated with a name, an email (since that will often always include the first name or last name), or an address. For example, if we’re talking about a male, under 21, living at 59 Bolton Square, it may be obvious who the person we’re talking about is.

Even if you don’t live in Europe, you’ve probably felt the impact of the EU’s **General Data Protection Regulation (GDPR)**. For example, if you’re US-based, and you’re dealing with a client that lives in the EU, the regulation still gives that client protection, with the thread of eye-watering fines to back it up. There are many other local equivalents (California in the US being the most notable).

The regulations are pretty well written and it’s easier to start following them from day 1. Up to now, many of your decisions and personal data have been kept internal to your company (in an Excel sheet). The GDPR and data protection regulations still apply, but you’ll notice that we’ve started passing data to external services (just now IBM/Red Hat, and shortly Microsoft). This makes the topic of the GDPR even more important.

The good news is that the approach that this book suggests is likely to make you more GDPR-compliant:

+   - We talked in the last chapter about not using specific personal information in business rules. Since we should only pass in the information that we need to make a decision, it is far less likely that our system will be impacted by the GDPR.