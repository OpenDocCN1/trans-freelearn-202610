
![](img/76a9a15ee6002f74a917dd7fbdfc8b1c_285_0.png)

CiviCRM is also currently limited in the fact that it can only attribute a single contribution payment to memberships or event registrations. However, work is currently being done to extend the data model to accommodate partial payments and other bookkeeping and accounting enhancements. This could potentially be used to record multiple distinct payments, each with their own contribution type, against a single "purchase", such as an event registration. Watch the CiviCRM blog for more information as this functionality is developed.

## Configuring payment instruments

CiviCRM stores information about how payments are made as payment instrument information. Payment instruments are the method of payment, such as checks, cash, EFTs (electronic funds transfers), credit cards, and debit cards. Your organization most likely does not support all of these default instruments for accepting payments.

To review and adjust payment instruments:

- 1. Click on **Administer | CiviContribute | Payment Instruments**.
- 2. To **Disable**, or **Delete** a payment instrument, click the respective link to the right of the relevant payment instrument.
- 3. To edit a payment instrument, click the **Edit** link to the right of its name, then enter or adjust the values in each field, and click on **Save**.

## Configuring accepted credit cards

There is a separate interface for specifying what credit cards may be accepted (for example, Visa, Amex). Do not confuse payment instruments with individual types of credit cards. In particular, do not create a separate payment instrument for each one. Instead, do the following:

- 1. Click on **Administer | CiviContribute | Accepted Credit Card Options**.
- 2. To **Disable**, or **Delete** a credit card option, such as Amex, click on the respective link to the right of the relevant credit card option.
- 3. To add a type, click on the **Add Accept Credit Card** button and enter the relevant information.

Be sure to review your payment processor options and limitations when setting up your system to confirm which card types they accept.

## Configuring a payment processor

In order to accept payments online, CiviCRM requires you to configure a payment processor, as discussed earlier in this chapter. Different payment processors have different parameters, options, and setup instructions. To determine how to set up and test your account with specific payment processors, visit http://wiki.civicrm.org/confluence/display/CRM_DOC32/CiviContribute+Payment+Processor+Configuration, click on the name of your payment processor, and follow the relevant instructions, or contact the processing company for more guidance.

> In future versions, payment processor integration will be handled through an extension mechanism. You may need to search elsewhere on http://wiki.civicrm.org for the name of your payment processor as that system is implemented.

For configuring CiviCRM to work with the payment processor, follow these steps:

- 1. Click on **Administer | Configure | Global Settings | Payment Processors.**
- 2. Enter a **Name**, perhaps a **Description**, and the **Processor Details** for the **Live** and **Test Payments** as per the instructions for your payment processor.
- 3. Click on **Save.**

If you have chosen a payment processor that requires your site to have an SSL certificate, you will need to purchase and install one. If you are using a hosting provider, they are likely to suggest preferred certificates and offer installation services for a fee. If you do not have a managed hosting arrangement that provides this service, you will need to have someone with system administration skills and sufficient server access install it. The technical procedures vary between operating systems and by certificate issuing authority, and are non-trivial for those lacking system administration experience.

Low cost and mid-range SSL certificates support only a single domain or sub-domain. This includes distinguishing between the root and www form of a domain. For example, a certificate for https://www.mydomain.org will not work for https://mydomain.org and vice-versa. You can purchase and install certificates for both domains, or buy a higher priced wildcard certificate that covers both.

- 1. Run a test payment or two for a small amount on a contribution page which can be configured as described in the *Creating an online contribution page* section towards the end of this chapter.
- 2. Ensure that the payments are received by the payment processor.
- 3. Verify that the funds are deposited properly in your bank account, either automatically or manually as the case may be.

Note that PayPal and some other payment processors have sandboxes that are more difficult to use than their production systems, so it can be faster and less problematic to just configure and test using real payments against their live servers.

It is also a good idea to have your bookkeeper review things at this point to make sure they can properly integrate online payments into your accounting procedures. Sometimes, there are issues with how payment processors batch transactions from online payments with those from point of sale terminals that can make things difficult to track. You may need to review how specific transactions stored in CiviCRM can reliably be correlated with deposits in your bank account and vice-versa. To review a report that may prove helpful in this regard, click on **Reports | Bookkeeping Transactions Report.**

## Configuring premiums

Premiums are thank-you gifts that may be provided to donors to encourage them to donate. They are often used as an incentive to give more. For example, anyone giving $50 can receive a coffee mug with your organization's logo, while those giving $100 can choose to get a T-shirt with the logo. All premiums are specified on a site-wide basis before being enabled for particular CiviContribute pages. Constituents donating through premium-enabled contribution pages will always have the option of declining premiums. By making these gifts optional, you reduce wasted resources on unwanted premiums.

There is no need to configure premiums in order to use CiviCRM for fundraising, and if you are just getting started, you should likely focus on more essential basic functions. As particular appeals begin to make use of premiums or new premiums, you will need to configure or reconfigure your site-wide premium options before they can be enabled for particular pages. Here is how to add a premium:

- 1. Click on **Administer | CiviContribute | Premiums (Thank You Gifts)** or **Contributions | Premiums (Thank You Gifts)**.
- 2. Click on **Add Premium**.
- 3. Enter information in the fields as it should be displayed to potential donors, starting with **Name**.
- 4. **Description** can be promotional text or just an explanatory description.
- 5. **SKU** is useful if the item is for sale elsewhere on your site.
- 6. For **Premium Image**, use the display image and thumbnail from these locations on the web option to display product images from your own site (for example, in a Drupal Ubercart store), as this will prevent them from getting out of sync when the store images are updated.
- 7. **Minimum Contribution Amount:** CiviCRM will enforce this minimum wherever the premium is enabled, displaying an error message if someone asks for the premium but doesn't contribute at or above this amount. You can create another premium for the same item if you would like to set a different minimum contribution amount for it on a different contribution page.
- 8. **Market Value:** Some jurisdictions require tax-deductible charitable receipts to be issued only for amounts that exclude the fair market value of any premiums provided. If that applies to your organization, and this premium will be used on pages accepting tax-deductible payments, enter a value here.
- 9. **Actual Cost of Product:** Enter a value here to improve the calculation of the net return from offering the premium and also the overall appeal.
- 10. **Options:** Provide a comma-separated list of options, such as size or color. The comma-separated list ignores line breaks, so text after the last comma on one line is put together with text at the beginning of the next until a comma is encountered. If you want to provide options for two aspects of a product, such as both color and size, you either must enter every combination as a separate option or do some custom programming.
- 11. **Subscription or Service Settings.** Custom programming can allow the information collected here to be used to provide temporary access to downloadable files such as reports or songs in your CMS or to memberships or subscriptions managed by CiviCRM. Alternatively, staff can manually administer such benefits once they receive an e-mail or other notification of a donation.
- 12. Click on **Save**.

You can return to add, modify, disable, or delete premiums at any time. If premiums are shared across multiple contribution pages, be sure to understand the impact of any modifications before you begin editing them.

## Configuring price sets

Price sets are used to construct more complex sets of donation-level options. In most cases, you will create a simple list of donation levels and include the option for donors to enter their own custom amount. However, if you require multiple sets of options, or quantity-based purchasing, price sets can be used.
