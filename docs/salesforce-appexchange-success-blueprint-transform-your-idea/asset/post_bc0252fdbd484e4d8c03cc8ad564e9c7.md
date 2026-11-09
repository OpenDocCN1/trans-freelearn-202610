
search and may offer better access control. Here, you can view intricate details about the subscribers and their org setup.

- **Request and obtain login access**: If a direct login to the subscriber’s org is required for issue resolution, you can request access using the console. Subscribers can also proactively grant access from within their own org by navigating to **Settings -> My Personal Information -> Grant Account Login Access** and selecting the name of your AppExchange product. For security reasons, the maximum period for granting access to ISV partners is 1 month. Once granted, you can log in as a subscriber and view their configuration and data to assist with problem resolution.
- **Log in to subscriber orgs**: Login to subscribers’ orgs is governed by user permissions and may require **multi-factor authentication (MFA)**. This requirement enhances security by verifying the identity of the user accessing the subscriber’s org.

While exploiting the extensive functionalities offered by the Subscriber Support Console, certain best practices can enhance its efficacy and ensure a secure troubleshooting process, as follows:

- **Creating an audit trail**: Logging a case in the **License Management Organization (LMO)** before each login to a subscriber’s org will create a trackable record. This trail provides accountability and transparency about when and why access to the subscriber org was needed.
- **Access control**: To preserve the security of customer data and configurations, limit login access to trusted support and engineering personnel. This practice safeguards subscriber’s orgs, as logging in grants full read/write access to customer data and configurations.
- **Utilizing debugging tools:** Use Apex debug logs to troubleshoot specific issues. You can generate these logs to view output from your managed package, which can assist in troubleshooting issues particular to that subscriber. Also, you can view and edit data contained in protected custom settings from your managed packages when logged in as a user.

## Maximizing revenue with AppExchange Checkout

![](img/44ed632038957e4c80220676e84c5109_459_0.png)

As we continue our expedition in the Salesforce AppExchange landscape, imagine stumbling upon the valley of AppExchange Checkout, a picturesque scene dotted with automated cash registers. Now, these aren’t your ordinary registers; they’re sophisticated tools that smooth out and automate the purchase process for your customers right from your AppExchange listing. It’s as if we’ve set up a bustling open-air market in the middle of this vast ecosystem.

But as with any market setup, it’s essential to know the lay of the land. You’ve got to ensure that your product is neatly packaged (distributed as a managed package) and that your booth fits the general market setup because Checkout cannot be used with OEM apps. This section is your tour guide, leading you through the market stalls, pointing out the nuances of AppExchange Checkout, and offering tips and tricks to make your shop the go-to destination for all.

So, roll up your sleeves, grab your register, and let’s set up the best market stall in the valley of AppExchange Checkout, making the buying process a gentle breeze for our dear customers. What do you say? Ready to start setting up?

## AppExchange Checkout – operational efficiency with automation

Built on Stripe’s industry-leading platform, AppExchange Checkout empowers customers to pay for your solution via credit card or bank transfer, thereby enhancing their buying experience. Alongside this, you can incentivize purchases with coupons and trials while managing **value-added tax (VAT)** and US sales tax.

A significant feature that bolsters operational efficiency is automatic license provisioning. Every purchase made via Checkout triggers the creation of a license record in the LMA. All actions, including upgrades, renewals, or cancellations, are automatically updated in the license, reducing manual intervention and potential errors.

Pairing AppExchange Checkout with the CMA provides further operational benefits. The CMA brings Salesforce CRM’s capabilities to Checkout, providing insights into revenue, subscription status, and more. It also enables sending customizable notifications for key events such as trial expirations and declined payments.

## Financial implications and revenue sharing

As of 2023³, revenue sharing with Salesforce varies based on the payment method. For bank transfers, it’s usually 15%, depending if you participate in the Marginal PNR program described in *Chapter 1*. For credit card payments, at the time of writing this book, it’s 15% plus a Stripe-imposed $0.30 per transaction fee. For instance, if you sell an app at $50 per user per month and a customer buys 10 licenses via bank transfer, your share with Salesforce would be $75 per month out of the total monthly transaction of $500.

3 You can confirm if the Checkout pricing has remained the same at this link: https://developer.salesforce.com/docs/atlas.en-us.packagingGuide.meta/packagingGuide/appexchange_checkout_rev_share.htm

Checkout supports both one-time and subscription payment plans. You can charge on a per-user or per-company basis, providing flexibility in your pricing strategy. While a one-time payment charges customers once at the time of purchase, subscription plans charge customers on a recurring basis, either monthly or annually.

## Applying AppExchange Checkout to your business

Implementing AppExchange Checkout requires certain steps. First, you need to set up an account with Stripe. Following this, a product and pricing plan should be created in your Stripe dashboard, setting the cost, currency, and billing frequency for your solution. Bank transfers can be enabled by requesting the payment method in Stripe. Upon approval, you can start receiving bank payments.

You can then enable Checkout on your AppExchange listing. You can also preview the customer experience by modifying the AppExchange listing URL. To cater to international customers, ensure your company is in a country supported by Stripe. If required by your local tax authority, enable VAT in the Publishing Console.

For heightened security, Checkout supports **Strong Customer Authentication (SCA)**, an identity verification step required for online payments in the **European Economic Area (EEA)**.

## Managing subscriptions effectively

AppExchange Checkout allows you to effectively manage subscriptions. You can handle customer requests related to Checkout subscriptions, such as viewing payment history, adding or removing licenses, and canceling subscriptions. This leads to improved customer service and satisfaction.

## Limitations of using AppExchange Checkout

While AppExchange Checkout offers a streamlined and automated solution to ease the purchasing experience for your customers, it’s essential to recognize its limitations. These include the following:

- **No varied price points for a single listing**: One of the most significant limitations of AppExchange Checkout is its inability to support varied price points for a single listing. This poses a challenge for businesses that operate on a freemium model or those that have multiple pricing tiers. Businesses looking to offer various packages or tiers will need to find alternative ways to present their pricing or consider separate listings for each price point.
- **Inflexible deal customization**: While AppExchange Checkout automates the payment process, this automation comes at the cost of flexibility. It becomes challenging to tailor deals based on a customer’s unique circumstances. For instance, you’ll encounter the following limitations:
  - **Delayed billing**: You can’t offer an agreement with billing postponed to a future date.
  - **Contract modifications**: If a customer’s legal department has specific requirements, modifying contract terms via Checkout might not be feasible.
  - **Additional features or support levels**: Upselling or customizing features and support levels for individual customers is not directly supported.
