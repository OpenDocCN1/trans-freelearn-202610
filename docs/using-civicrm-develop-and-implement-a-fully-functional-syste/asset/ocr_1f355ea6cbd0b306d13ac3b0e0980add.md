![](bbox=[0.21, 0.2, 0.26, 0.24])

CiviCRM is also currently limited in the fact that it can only attribute a single contribution payment to memberships or event registrations. However, work is currently being done to extend the data model to accommodate partial payments and other bookkeeping and accounting enhancements. This could potentially be used to record multiple distinct payments, each with their own contribution type, against a single "purchase", such as an event registration. Watch the CiviCRM blog for more information as this functionality is developed.

# Configuring payment instruments

CiviCRM stores information about how payments are made as payment instrument information. Payment instruments are the method of payment, such as checks, cash, EFTs (electronic funds transfers), credit cards, and debit cards. Your organization most likely does not support all of these default instruments for accepting payments.

To review and adjust payment instruments:

+   1. Click on **Administer | CiviContribute | Payment Instruments**.
+   2. To **Disable**, or **Delete** a payment instrument, click the respective link to the right of the relevant payment instrument.
+   3. To edit a payment instrument, click the **Edit** link to the right of its name, then enter or adjust the values in each field, and click on **Save**.

# Configuring accepted credit cards

There is a separate interface for specifying what credit cards may be accepted (for example, Visa, Amex). Do not confuse payment instruments with individual types of credit cards. In particular, do not create a separate payment instrument for each one. Instead, do the following:

+   1. Click on **Administer | CiviContribute | Accepted Creditcard Options**.
+   2. To **Disable**, or **Delete** a credit card option, such as Amex, click on the respective link to the right of the relevant credit card option.
+   3. To add a type, click on the **Add Accept Creditcard** button and enter the relevant information.

Be sure to review your payment processor options and limitations when setting up your system to confirm which card types they accept.