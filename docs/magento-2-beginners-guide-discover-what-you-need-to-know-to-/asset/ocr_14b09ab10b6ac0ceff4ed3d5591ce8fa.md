# Processing Payments

Magento includes payment gateways that securely transmit credit card data through direct API post methods or hosted payment forms provided by the payment gateway.

The following are the requirements to meet the **Payment Card Industry Data Security Standard (PCI DSS):**

+   1. Install and maintain a firewall configuration to protect cardholder data.
+   2. Do not use vendor-supplied defaults for system passwords and other security parameters.
+   3. Protect stored cardholder data.
+   4. Encrypt the transmission of cardholder data across open public networks.
+   5. Use and regularly update your antivirus software.
+   6. Develop and maintain secure systems and applications.
+   7. Restrict access to cardholder data to business need-to-know.
+   8. Assign a unique ID to each person with computer access.
+   9. Restrict physical access to cardholder data.
+   10. Track and monitor all access to network resources and cardholder data.
+   11. Regularly test security systems and processes.
+   12. Maintain a policy that addresses information security.

There are two methods to protect your customers and guarantee PCI compliance:

+   - **Direct post:** The information is sent directly to the payment gateway without data flowing through or being stored on Magento.
+   - **Hosted payment form:** The payment form is integrated in the Magento checkout, but it is hosted by the payment gateway rather than by the Magento application.

By keeping sensitive data outside the Magento instance, merchants can validate compliance through self-assessment at the SAQ A or SAQ A-EP level without having to go through the PCI compliance reassessment of the entire Magento platform.