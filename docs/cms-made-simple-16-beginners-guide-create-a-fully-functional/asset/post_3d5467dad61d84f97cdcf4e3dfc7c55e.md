
Install the module **Paypal Gateway** from the list of the available modules in **Module Manager**. The admin area of the module is placed in **Extensions | Paypal Gateway**. Click on the **Preferences** tab. There are some fields that are required for module usage, which are shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_269_0.png)

While testing e-commerce solutions you are naturally not going to use your real PayPal account. The suggested **Paypal URL** points to the official testing environment of the PayPal service. This environment simulates the payment process without making real money transfers.

This environment can be found on `http://sandbox.paypal.com`. It acts the same way as the real PayPal website does.

## Time for action—creating test accounts

Before you use this environment, you have to register a developer account on `http://developer.paypal.com` and create two test accounts.

- 1. Open `http://developer.paypal.com` in your browser and click on **Sign Up Now**.
- 2. Fill in the fields and click **Agree and Submit**.
- 3. Log in to your developer account with your e-mail and password.
- 4. Click on **Create a preconfigured buyer or seller account**.
- 5. Select **Buyer (Use to represent your customer's experience)** and fill in the field with any test data. Click **Create Account**.
- 6. Create another account, but this time choose **Seller (Use to represent yourself as the merchant)** as the **Account type**.

**7.** After both accounts are created, click on **Test accounts** on the left hand side. You should see an overview of the test accounts as shown in the following screenshot:

![](img/b21e3dc5c94405bc7b3f102758ea770c_270_0.png)

## What just happened?

You have created two test accounts, one for the buyer and one for the seller. The easiest way to create such accounts is to choose the preconfigured ones. Now, you can log in with the buyer account on `http://sandbox.paypal.com` and act as if you were your own client. You can purchase anything without having to pay real money.

You can also log onto http://sandbox.paypal.com with the seller account to configure it. You have to activate the PDT and IPN features for the account, get your PDT identity token, and make some more settings on your PayPal account, so that instant payment works with the module PayPal Gateway.

## Configuring PayPal's seller account

Click on the Profile link in the seller account. There are four sections that you have to customize to set up PayPal payment with the website.

![](img/b21e3dc5c94405bc7b3f102758ea770c_271_0.png)

## Payment Receiving Preferences

First of all, decide if you would like to manually accept payments made in other currencies. This applies only if the default currency of your PayPal account is different to the currency set in the Products module on the Preferences tab. For example, if your PayPal currency is Euro but on your webpage you offer the products in USD. Generally, PayPal does not convert any currency automatically. This means that all payments made in other currencies are on hold until you accept them in your PayPal account manually. Once the payment is accepted the order status is set to Paid. If you would like to convert currencies automatically and accept payments in any currency, you have to deselect the setting Block payments sent to me in a currency I do not hold.

Secondly, you can block all payments with the status Pending. This will reject all payments that are not completed instantly after the payment confirmation. Consider that the Paypal Gateway module is able to handle pending payments if IPN is activated.

This is your generated PDT identity token that you need to save in the **Paypal Gateway** module on the **Preferences** tab.

## Language encoding

On this page you should adjust the language encoding. CMS Made Simple uses UTF-8 encoding by default. Click on the **More Options** button and select UTF-8 in the **Encoding** field. Leave the same encoding for IPN and save.

Now you have all the information required to configure the **Paypal Gateway** module. In the admin area of the **Paypal Gateway** module, click on the **Preferences** tab and enter your custom values gained from PayPal sandbox environment. The **Business Email** field in the **Paypal Gateway** module contains the generated e-mail address of the seller account. The following screenshot shows my custom values:

![](img/b21e3dc5c94405bc7b3f102758ea770c_273_0.png)

Next, connect the **Orders** module with the **Paypal Gateway** module. In the admin area of the **Orders** module, click on the **Preferences** tab and scroll down to the **Payment Gateway Settings** section. Choose the **Paypal Gateway** module in the list of payment modules and click **Submit**.

Test your order process now. Pay attention to the **Email** field in the billing form, it must be filled in for the PayPal payment. After the payment is made, you have to control the payment status in the seller PayPal account. It should be completed. In the admin console of CMS Made Simple, click on **Site Admin | Admin Log** and see the communication protocol with the PayPal website. For each order completed with PDT and IPN you should have five messages, as shown in the next screenshot:

| User | Item ID | Item Name | Action |
|---|---|---|---|
| webmaster | 0 | PaypalGateway | Received successfull transaction reply for 12Y48348G3951362M |
| webmaster | 0 | Orders | Order status changed to "Pending" on 16 |
|  | 0 | PaypalGateway:ipn | IPN Transaction 12Y48348G3951362M successful. Status: Completed |
|  | 0 | Orders:gateway_event | Handled Async Transcation 12Y48348G3951362M for order 16 |
|  | 0 | Orders | Payment received for order 16. |

The reason for the order being pending is the activated IPN feature. The module sets the order to pending status until the IPN message is received from the module. So, the result of these five messages above is a completed and paid order in the admin console of CMS Made Simple.

Once you have tested the module, you should configure your real PayPal account as explained previously, gather all required information like PDT identity token from the real account, and change the PayPal URL to https://www.paypal.com/cgi-bin/webscr.

## Optional modules for the e-commerce suite

There are some more modules that you can use together with this e-commerce suite to enrich the functionality:

- **Flat Rate Taxes**: Allows you to apply flat rate taxes to any or all products in the module **Products**.
- **warehouse taxes**: Calculates taxes for a product based on the US state the warehouse is in, and the US state the customer is situated.
- **FlatRateShipping**: Provides an interface to set up some simple weight shipping rules, and to calculate the cost of shipping based on its total weight.
- **Destination Based Shipping**: Allows the calculation of flat shipping costs based on destination country, and number of items. It does not consider weight in calculations.
- **PriceBasedShipping**: Allows the calculation of shipping costs based on the pre-tax total order price.

- 4. If you would like to test PayPal functionality on your website, you can use a so called sandbox environment. What is true about the PayPal sandbox?
  a. It charges your real PayPal account.
  b. It can be used to create a seller account for testing purposes.
  c. It can operate only with USD as currency.
  d. It is not free

- 5. E-commerce solution described in this chapter requires that the customer creates an account before purchasing:
  a. True
  b. False

## Summary

In this chapter, you learned how to set up basic e-commerce functionality with CMS Made Simple. Even if CMS Made Simple does not pretend to offer fully sophisticated e-commerce features, it offers the most wanted features with delivered modules.

Be sure that you do not use CMS Made Simple for pure e-commerce websites. It is better to use a dedicated e-commerce solution for an online shop rather than CMS Made Simple. However, if your website has a mix of features such as a blog, a shop, a forum, and so on, then you can use third-party modules to implement these features.

Specifically, we covered:

- The **Products** module that allows you to create and edit products for the online shop. This module is a basis for the e-commerce suite.
- The **Cart** module that adds simple cart functionality to the **Products** module. It saves its data in the browser session and connects the module **Product** with the module **Orders**.
- The **Orders** module that starts the checkout process for the items added to the cart. This module saves all orders in the admin area and allows tracking and updating order status for the administrator of the website.
- The **SelfRegistration** module that allows your website's visitors to register with your website before making payment. We have also created a small workflow to see how the module is integrated in the checkout process.
- The **PayPal Gateway** module that accepts instant payments from the PayPal website and keeps you informed about the status of the payment.
