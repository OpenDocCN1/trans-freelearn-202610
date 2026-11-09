
- **Multi-year contracts:** Checkout doesn’t natively support longer-term relationships through multi-year contracts.
- **Geographical restrictions:** AppExchange Checkout relies on Stripe, which means your business must be in a country supported by Stripe. While Stripe has a wide reach, there are still numerous countries where it isn’t available. This limitation might hinder businesses aiming for a global audience.
- **Complex tax implications:** Although the system manages VAT and US sales tax, businesses with complex tax requirements or those operating in countries with intricate tax systems might find the automated solutions insufficient.
- **Over-reliance on Stripe:** Since Checkout is built on Stripe’s platform, any changes, updates, or issues with Stripe can have a direct impact on your Checkout experience. It’s vital to stay updated with Stripe’s policies and updates.

While AppExchange Checkout offers many benefits, it’s crucial for businesses to weigh these against the limitations. Depending on your business model, customer base, and geographical focus, AppExchange Checkout might be the perfect solution, or you might need to consider alternative methods, described later in this chapter, or supplementary methods to cater to your specific needs.

## Leveraging the CMA for operational excellence

As an ISV Partner, a vital part of operational excellence is the effective management and monitoring of your product offerings. Salesforce provides a powerful tool for this purpose: the **Checkout Management App (CMA)**. The CMA integrates with AppExchange Checkout, presenting valuable data on a user-friendly dashboard and enabling automated email notifications for key activities. Here, we explore the CMA in depth.

## Harnessing the power of the CMA dashboard

The CMA dashboard offers a comprehensive view of your AppExchange Checkout data. Preconfigured displays include the following:

- **Monthly revenue**: Tracking financial performance becomes effortless, aiding in financial forecasting and planning
- **New subscribers per month**: Growth trends become visible, informing marketing strategies and customer acquisition efforts
- **Subscription plan by unit**: Reveals customer preferences, offering insights for product development
- **Monthly subscription status**: Monitors trials, purchases, and renewals, enabling proactive customer engagement

The CMA dashboard can be customized using standard Salesforce tools, allowing you to delve deeper into customer data, subscription plans, invoices, and transactions for more precise analytics.

## Streamlining communication with email notifications

The CMA enables automated email notifications for typical partner situations, including renewal notices. You can enable, customize, and use these notifications as per your requirements, reflecting your company’s brand and identity while staying connected with customers and team members.

## Getting started and customizing the CMA

Implementing the CMA involves installing it into your PBO, configuring data access levels, and enabling email notifications as required. You can then customize the CMA further to meet your specific needs. For example, notifications can be augmented with links to additional resources such as setup documentation, or reports can be modified to show annual revenue for a specific offering instead of the default monthly revenue.

## Managing CMA settings for enhanced control

Admin users have the power to manage when and to whom the CMA sends emails, change the associated Stripe account, and reimport data manually into the Salesforce org. Furthermore, you can configure logs in the CMA to assist with troubleshooting, providing an additional layer of control.

## Efficient channel order management with the COA

![](img/44ed632038957e4c80220676e84c5109_467_0.png)

Embarking on the journey of operational excellence, the path of an AppExchange Partner resembles the trek of an ambitious mountaineer navigating the COA mountain range. Picture the COA as our go-to climbing gear—reliable, trustworthy, and crucial for scaling these majestic heights. This tool isn’t just a hiker’s accessory; it’s an efficient command center for managing orders.

This section is the Sherpa guiding you through the often rocky but rewarding terrain of efficient order management. We’ll walk you through how to pitch your base camp (creating orders), how to plan your ascent (managing orders), and when to radio in for assistance (fixing errors in orders). Just as the thrill of reaching the peak is in overcoming complex climbing challenges, we’ll also be untangling complex operational knots along the way.

So, buckle up your harness, check your climbing gear, and prepare to ascend the COA mountain range, from where your operations will stretch out beneath you, as orderly and awe-inspiring as the view from the summit.

## What is the Channel Order App (COA)?

The COA is a pre-installed application in your PBO that enables efficient order processing. Its utility varies based on the type of partnership, as outlined here:

- **For OEM:** The COA aids in provisioning Salesforce licenses and in revenue-sharing arrangements
- **For ISVforce:** The COA primarily supports revenue sharing

It’s important to note that partners using AppExchange Checkout for managing customer payments should not utilize the COA, as revenue reporting in such cases is automatically handled when customers purchase your AppExchange solution.

## Training and access

Given the critical role that the COA plays in order management, Salesforce requires all AppExchange partners to undergo training before using it. This training, provided by the Partner Operations team, ensures that ISVs understand how to leverage the full functionality of the COA. Therefore, the initial steps toward using the COA involve acquiring your PBO, passing the solution security review, and signing up for COA training. This chapter assumes you’re familiar with the basics from the mandatory training and aims to dive deeper into the COA’s potential for handling complex scenarios and integrating them into your current workflows.

## The significance of the COA

After the conclusion of sales to end customers, all orders need to be relayed to Salesforce through the COA. This platform allows partners to place customer orders and report their sales to Salesforce. Following the receipt of your order, Salesforce takes the responsibility of activating or provisioning the product within your customer’s org. This process ensures revenue sharing is tracked in line with the terms of your Salesforce partnership agreement.

## Order types in the COA

Every application subscription sold to an end customer mandates that an order is submitted to Salesforce via the COA. Such orders must be lodged within a day of receiving the corresponding customer order. The COA offers a range of order types based on various customer scenarios. These include the following:

- **Initial Order:** This is the first order submitted for a fresh customer
- **Add-On:** This pertains to the addition of products or an increase in license numbers for an existing customer
- **Upgrade:** This involves increasing the cost of existing licenses mid-contract or upgrading a customer to a more expensive product during the contract
- **Renewal:** This applies to the renewal of an existing contract not slated for auto-renewal or involves a change in the price of existing licenses upon contract renewal
- **Reduction:** This involves the removal of one or multiple products or a decrease in license numbers on a customer contract
- **Cancellation:** This refers to the permanent removal of all products at the conclusion of a customer contract
- **New Cloud:** This relates to placing an order for an additional, distinct cloud service for an already-established customer

## Picking the right order type

The following questions will help you determine the correct order type based on key variables, such as customer status, license quantity adjustments, effective dates, and pricing modifications:

- Is your order for a new or existing customer?
- When do you want the order to be effective?
- How are you adjusting the license quantities?
- What is the new license pricing?
- How are you adjusting current license pricing?
