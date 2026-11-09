
## Leveraging the AppExchange Partner Toolkit

As an AppExchange partner, having the right tools at your disposal is instrumental in navigating your journey successfully. Just as a seasoned adventurer wouldn’t venture into the wilderness without a reliable compass, a sturdy pair of boots, and a well-stocked backpack, you wouldn’t want to delve into the dynamic landscape of the AppExchange marketplace without your own set of tools.

In this section, we are going to unveil the AppExchange Partner Toolkit, an assortment of resources designed to empower partners such as you. The aim is to familiarize you with the key tools available, each serving its unique purpose, to help you streamline your operations, manage your customers, and effectively deploy your applications.

Armed with the knowledge of these tools, you’ll be well-equipped to scale the heights of the AppExchange ecosystem. So, let’s embark on this exciting expedition to uncover the AppExchange Partner Toolkit.

## PBO features and capabilities

Dear reader, welcome aboard the magical mystery tour of the PBO, your very own Salesforce command center, where dreams of managing your business operations come true. No need to pinch yourself—this is real, and it’s time to explore the PBO’s features.

Your PBO is your versatile sidekick, ready to do the following:

- Woo new customers with Salesforce CRM apps
- Manage scratch orgs and second-generation managed packages efficiently with the **Developer Hub (Dev Hub)**
- Create test orgs using the Environment Hub
- Keep your leads and licenses in check with the **License Management App (LMA)**
- Manage application features effectively with the **Feature Management App (FMA)**
- Slay the order submission game with the **Channel Order App (COA)**
- Seamlessly manage payments with Checkout
- Be the superhero your customers need with the Subscriber Support Console

## The Environment Hub – your secret laboratory for app testing and packaging

![](img/44ed632038957e4c80220676e84c5109_105_0.png)

Picture this: you’re a mad scientist (in the most endearing way, of course) concocting the perfect AppExchange potion. Your laboratory? The Environment Hub. It’s here that you can perfect your app’s recipe by creating test orgs, streamlining packaging, fostering collaboration, and keeping track of all your experiments. Let’s dive into the benefits of this fantastic laboratory of yours.

Here are some benefits of using the Environment Hub:

- **Easily create test orgs:** Much as with a master chef testing out different ingredients, the Environment Hub allows you to whip up test orgs for various Salesforce editions and configurations. This ensures your app is fully compatible and performs optimally in every kitchen (ahem, environment) it lands in.
- **Streamline app packaging:** Consistency is key, whether it’s in your secret sauce or app packaging. The Environment Hub helps ensure that your app is packaged uniformly across environments, making it a breeze to deploy and distribute.
- **Improve collaboration:** Cooking is always more fun with friends. The Environment Hub lets you share environments with your team members, making the testing process a collaborative and efficient endeavor. Together, you’ll identify and resolve issues faster than a soufflé can rise.
- **Centralized tracking:** Keep your laboratory organized with the Environment Hub’s centralized view of all your environments. Easily track vital information such as creation date, edition, and expiration date, ensuring your environments stay up-to-date and compliant with Salesforce requirements.

## Accessing the Environment Hub – finding your way to the lab

Ready to put on your lab coat and goggles? Here’s how to access the Environment Hub:

- 1. Log in to your PBO.
- 2. Click the **App Launcher** icon in the top-left corner.
- 3. Search for **Environment Hub** and select it from the search results.

Now that you’ve found your way to this secret laboratory, it’s time to channel your inner mad scientist and start experimenting.

## LMA – your personal bouncer for app licenses

![](img/44ed632038957e4c80220676e84c5109_108_0.png)

You’re going to be the proud owner of an exclusive club (your app) on the bustling AppExchange street. The LMA is your trusty bouncer, keeping track of all the guests (licenses) entering and leaving your establishment. In this section, we’ll learn about the benefits of having the LMA as your bouncer, give you a tour of the **License Management Org (LMO)**, and explain the relationship between the LMO and your PBO.

## Overview of the LMA

The LMA is designed to keep your club’s guest list (licenses) in check, and with the LMA, you can do the following:

- 1. Monitor and manage your clientele, including those on trial visits and VIPs with paid access.
- 2. Control the guest list by enabling or disabling licenses like a velvet rope.
- 3. Track customer attendance and app popularity.
- 4. Access customer orgs for support purposes using the Subscriber Support Console.

The LMA incorporates three custom objects: Package, Package Version, and License. The Package object serves as the foundational entity, encompassing all LMA data. Each Package object can encompass multiple Package Versions, which in turn can be associated with numerous Licenses. Every License corresponds directly to a single Lead. Notably, when a Lead undergoes manual conversion into an Account and Contact, the associated License seamlessly links to the newly converted records. The following **entity relationship diagram (ERD)** provides a visual representation of these intricate connections between custom and standard Salesforce objects:

![](img/44ed632038957e4c80220676e84c5109_110_0.png)

Figure 2.2: Entity relationship diagram of LMA system entities and their relationships

## The LMO

The LMO is like the exclusive backstage area of your Salesforce organization, where you can track all Salesforce users that install your managed package on Salesforce AppExchange. Whenever a user installs or uninstalls your package, the LMO gets notified with a lead record, like an autograph from each guest. The LMO tracks each package uploaded on Salesforce AppExchange and can be any Salesforce Enterprise or Unlimited organization that has installed the free LMA from AppExchange.

The best option is to have LMO installed in your PBO. This means that your PBO serves as the central repository for all license information related to your app installations, keeping everything neatly organized behind the scenes.

## FMA for managing app features

You can think of the FMA as a maestro ready to orchestrate a memorable experience for each guest at your club. We’ll explore the core concepts, use cases, and the two FMA parameters: LMO-to-Subscriber feature parameters, and Subscriber-to-LMO feature parameters.

## Understanding the FMA

The FMA is not mandatory to use, but it can be the mastermind behind your app, orchestrating the perfect blend of features and functionality for each customer. Built on top of the LMA, the FMA allows you to fine-tune **user experiences (UXs)** by controlling what users see and how they interact with your app. Consider the FMA the conductor of your app’s symphony, guiding every note to create a harmonious experience.

Here is an Entity relationship diagram, illustrating the relationships between various entities in the FMA:

![](img/44ed632038957e4c80220676e84c5109_111_0.png)

Figure 2.3: Entity relationship diagram of FMA entities and their relationships

## FMA use cases

The FMA can perform various tunes to cater to your guests’ preferences.
Here are a few examples:

- **Freemium model:** Customize features based on whether a client has a paid or free license type
- **Piloting betas:** Release new features to a select group of customers for testing without affecting current users
- **Streamlining development:** Reduce development time and overhead by managing multiple applications using the FMA, strategically aligning features to leverage the existing code base

## Enabling and disabling features with LMO-to-Subscriber feature parameters

LMO-to-Subscriber feature parameters are like the maestro’s wand, enabling or disabling features with a simple flick. These parameters are writable on your end and read-only for your subscribers, and can be used to control access to certain features or set limits on resource usage. Want to offer a sneak peek of a new feature? No problem! Enable it for a limited trial period and watch the excitement unfold. Just update the related feature parameters in your LMO, and let the magic happen in your code.

## Tracking preferences and activation metrics with Subscriber-to-LMO feature parameters

Subscriber-to-LMO feature parameters are the maestro’s attentive ears, listening to the feature activation preferences of your subscribers. With values assigned by your subscribers and sent to your LMO, you can collect insights and adapt your performance to their liking. Just update the feature parameters in your subscriber’s org using Apex code, and you’re good to go.
