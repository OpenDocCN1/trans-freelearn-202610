
## Data Regulatory Compliance

There are several data regulations in place based on industry standards and government regulations. These have been created to mitigate the risk of unauthorized access to corporate, personal, or government data. Some of the regulations that you may come across more frequently these days while designing a Salesforce solution include the following:

- General Data Protection Regulation (GDPR)
- United Kingdom version of GDPR (UK-GDPR)
- Health Insurance Portability and Accountability Act (HIPAA)
- California Consumer Privacy Act (CCPA)
- Fair and Accurate Credit Transactions Act of 2003 (FACTA)
- Act on the Protection of Personal Information (APPI)
- Gramm-Leach Bliley (GLB)
- Payment Card Industry Data Security Standard (PCI DSS)

Failure to comply with these regulations can result in hefty fines and public damage to the enterprise’s reputation, in addition to civil and criminal liability. The details of these regulations are not covered in this book, but you are encouraged to seek additional knowledge online and from other books. You will come across requirements that must be resolved, along with considerations regarding some of the previously mentioned regulations, in the chapters to come.

PCI DSS, also known as PCI compliance, is an information security standard that’s applicable to all companies involved in activities related to credit card information, such as accepting, processing, storing, or transmitting credit card information from credit card providers. Salesforce billing became PCI level 1 compliant in 2012 and has retained its compliance every year since.

You maintain compliance with PCI by never storing any credit card information in Salesforce at all payment method collection stages (before, during, and after). The payment card’s information should only be transmitted to the payment processors via a token and never stored within Salesforce. Each of these tokens is unique per customer, payment card, merchant, and payment processor. Once the token has been submitted, the payment processor can link it to the actual personal account number stored.

Using tokens, Salesforce can store a representation of the customer’s payment card without storing the actual details of the card itself. If the token falls into the wrong hands, it is useless because it only works when it is being used by the original merchant and payment processor.

The information that can be stored in Salesforce while still maintaining PCI compliance includes data such as the following:

- Name on card
- Last four digits of the credit card number
- Card type
- Token
- Expiration month and year

It is important to understand how online payment mechanisms work and how to select the right one for your solution. Suggesting a non-PCI-compliant solution during the review board could open the door to many other discussions that you do not want to have. When you suggest a suboptimal solution, the judges will try to understand the rationale behind your suggestion. Failing to show enough knowledge and understanding of regulations and compliance could lead to exam failure.

For online payments, you need to know that there are generally two main ways to integrate with a payment gateway. You can either create a checkout page that is hosted on your own website (for example, Salesforce communities) or utilize a checkout page that is provided and hosted by your payment gateway.

In terms of the externally hosted checkout approach, your checkout page is hosted on a different website provided by the payment provider. When the customer clicks on the checkout button on your website, they are redirected to the payment page, which is hosted on a different domain name. The order information and cart total amount will normally be shown on the payment page. The customer will enter their card details on this page and then hit the submit button. Notice that the card details are entered on the external payment website only; your pages and forms are never used to capture such details. Once the payment is successful, the customer is redirected to a landing page on your website. A confirmation or transaction code will also be passed back to your website.

The hosted checkout approach has the following advantages:

- **Enhanced security**: Every additional party involved in a payment transaction increases the risks associated with data breaches. You should aim to have as few data transfers as you can. Also, since the payment details are collected by the payment provider/processor, your website is not considered a part of the transaction, so the overall security level of this approach is high.
- **Less liability**: You reduce your liability as a merchant because you are not collecting any sensitive information. All of that is done by the payment provider.
- **Peace of mind for the customer**: Customers might find it more assuring to provide their payment details to a website/domain they already trust, such as PayPal. This is particularly true for smaller merchants or for newer brands.
- **Flexibility in payment methods**: Normally, hosted checkout pages provide the user with the choice to choose from multiple payment methods. This page will continue to be updated as the payment provider includes and more payment methods. You do not need to worry about that or change anything in your code.
- **Simplified setup**: This method is the easiest to set up and maintain.
- **Limited customization**: Although the payment page is hosted on another website, many of the payment providers allow a limited level of customization, such as setting a logo, header and footer, and color scheme.

Regarding the onsite checkout approach, the customer provides their payment details on a page hosted on their own website (for example, Salesforce communities). You have full control over the look and feel of this page, but you are also responsible for the implementation of every required functionality, validation, and security measure.

The onsite checkout approach has the following advantages:

- **Totally seamless**: This is probably the most important advantage of this approach. The customer is not redirected anywhere; the entire payment process feels like part of the journey and experience that you are providing to your customer.
- **One-click purchasing**: This is similar to what major merchants such as Amazon do. You are collecting the customer’s payment details so that you can attach them to their account. This opens the door to introducing one-click functionality.
- **Customization**: You can make the payment collection page in any way you like. It can be completely styled to meet your website’s look and feel. This goes beyond UI styling as you can, for example, collect additional details as part of the checkout process.

If you are taking payments via a hosted page, it is much easier to fill in and submit the required forms to stay PCI-compliant. This also makes the maintenance process simpler. If there is no clear need to go with an onsite checkout approach, then it is always safer to suggest an externally hosted page approach.

Now that you have covered some of the key topics you need to know about regarding data regulations, move on to understand the different data categories that are available.

## Exploring Data Categories

**Reference data** and **master data** are two common data categories that an architect would typically come across in most projects. In addition to these, in this section, you will also discover the characteristics of transactional data, reporting data, metadata, big data, and unstructured data. Having a deeper understanding of these different data categories will help you craft your overall data strategy, including data governance. This will also help you speak the same language your data architects prefer to use. Have a look at each of them closely.
