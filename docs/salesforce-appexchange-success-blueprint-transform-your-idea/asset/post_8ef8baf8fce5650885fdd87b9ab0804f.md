
## Report: Licenses
### Orgs by Major Release

Enable Field Editing

![](img/44ed632038957e4c80220676e84c5109_386_0.png)

Record Count

Package Version Number

- 0.13
- 0.18
- 0.39
- Other

| Package Version Number ↓ | License: License Name | Org Instance | License Status | Org Status |
|---|---|---|---|---|
| 0.5 (1) | L-00033 | NA162 | Active | FREE |
| Subtotal |  |  |  |  |
| 0.39 (112) | L-00018 | EU40 | Active | FREE |
|  | L-00347 | NA111 | Trial | FREE |

Figure 7.2 – Example of the “Orgs by Major Release” report from the LMA

*Figure 7.2* clearly shows the distribution of our customers across various versions of our application. Most of our customers are up to date, using the latest stable version (0.39). However, there is a small subset of users still operating on legacy versions (0.5, 0.13, and 0.18).

## How often should you release and deploy?

Now that you understand the differences between releases and deployments, as well as the power of push upgrades, the question arises: How often should you release and deploy?

The frequency of releases largely depends on several factors such as your development team’s speed, your customers’ adaptability to new changes, and your organization’s strategic goals. Many ISVs choose to release new features three times a year, aligning with Salesforce’s release cycle. This cadence allows ample time for development, testing, and ensuring compatibility with the Salesforce platform’s updates.

On the other hand, deployments can be performed as frequently as you want. They can serve as an excellent tool for delivering updates, bug fixes, and enhancements promptly. For example, during my tenure as a release manager at IPfolio (a substantial OEM), I introduced a weekly cycle of deployments. This approach ensured swift delivery of minor bug fixes and new feature-related code to all customers. However, since these features were not officially announced and were undocumented, only a select group of pilot customers had access to them.

This strategy enabled us to adhere to DevOps principles and maintain agility without the burden of maintaining multiple versions of our package. Yet, we still officially released new features only three times a year.

And remember—a release is not just about the technical aspects. It’s also about the human aspect, such as communication, providing release notes, offering training to your customers, and so forth.

By now, you should have a solid understanding of not only *why* but also *how* to design your release management process to best fit your organization’s needs. Balancing the frequency of releases and deployments effectively will ensure that your team can deliver continuous value to your customers while also managing change and maintaining stability.

## Why don’t you just release weekly?

“But can’t we just release new features every week?” I hear you ask. The answer is yes, you technically could. But should you? That’s a whole other kettle of fish.

Your end users, believe it or not, aren’t always thrilled by surprises. Sure—in a **Business-to-Customer (B2C)** scenario, rapid updates and changes might be acceptable, even exciting. But in the **Business-to-Business (B2B)** world, that’s not always the case. Imagine showing up for work on a Monday morning, coffee in hand, only to find that the tools you were using just last week have changed overnight. Yikes.

And it’s not just about managing the shock value. Do you have the bandwidth to offer adequate training, documentation, and support for these new features every week? Updating your product is one thing, but getting your users up to speed is a whole other challenge.

Frequent updates without proper communication and support can lead to a slew of issues. We’re talking about poor adoption of new features, skyrocketing support costs, and overall user dissatisfaction. Think of it this way—change management is an art form in and of itself. It exists because, generally speaking, people don’t really enjoy surprises at work.

So, yes—you should deploy your latest code frequently, but releases? Not so much. Just look at Salesforce for a real-world example: it deploys updates frequently but only has three major releases per year. So, while it’s tempting to show off your shiny new features as soon as they’re ready, remember that patience is a virtue, especially when it comes to software release management.

## Summary

![](img/44ed632038957e4c80220676e84c5109_390_0.png)

Great job, trailblazers! We’ve mastered release management, armed with our knowledge of version types and push upgrades. We’ve ensured our customers always have the latest tools, and we’ve learned to choose the perfect times to release and deploy. The result? A seamless software transition from design to end user, without disrupting live environments.

The practical skills you’ve gained are crucial for stability, risk minimization, and successful change management in your Salesforce ISV journey. It’s as important as starting a fire in the wilderness.

But we’re not done yet! Our next adventure is onboarding new customers. We’ll learn to create an attractive AppExchange listing, optimize trial experiences, speed up installations and upgrades, design automated onboarding processes, and manage local changes made by our customers. So, pack your bags, trailblazers. Our next adventure awaits! Let’s conquer customer onboarding together!

## Further reading

After navigating through the thrilling waters of release management, you may feel the call to deepen your knowledge and refine your skills further. To guide you on this quest, I have a compelling recommendation: *Mastering Salesforce DevOps* by Andrew Davis.

## Onboarding New Customers

![](img/44ed632038957e4c80220676e84c5109_392_0.png)

Ahoy, Salesforce explorers! As we dawn upon a new day in our software safari, we find ourselves facing another enthralling challenge—onboarding new customers! Picture it as inviting newly made friends to a house you’ve constructed. The grand tour should be captivating enough to make them want to move in and be long-term roommates, shouldn’t it? This chapter will provide you with the master blueprints to do just that.

In this part of our expedition, we will be discovering the following topics along the way:

- **Crafting a winning AppExchange listing**: Let’s start with the crucial part—a journey full of creativity, strategy, and the chance to make your app shine brighter than a disco ball at a ’70s party.
- **The art of onboarding**: Welcome new users and show them around your digital house. We’ll learn how to keep the tour engaging and downright fun because, let’s face it, no one remembers a dull party!
- **Instructions for installation**: Not all of us are as handy as *Bob the Builder*. We’ll discuss how to provide crystal clear instructions that even a caveman could follow to install your application.
- **Supporting local changes by your customers**: It’s like allowing your roommates to redecorate their rooms. We’ll delve into embracing customer customization and supporting local changes, without turning your application into a Picasso painting.
- **Crafting cinematic demos**: Imagine being a film director for a moment, where your application is the star of the show. We’ll uncover the secrets of creating demo videos that could win an *Oscar* for “Best App Introduction”!
- **Providing an interactive trial experience**: Ever wanted to test drive a spaceship? Well, with Test Drive and Trialforce, we’ll learn how to give your users the thrill of steering your app around their problem galaxy.

By the end of this chapter, you’ll be armed with navigational charts to create an effective onboarding process that encourages potential users to become full-time, happy dwellers in the house of your software.

Buckle up, brave explorers! Let’s plunge into the engaging world of onboarding new customers and inspire them to become permanent members of your Salesforce neighborhood!

## Crafting an effective AppExchange listing
