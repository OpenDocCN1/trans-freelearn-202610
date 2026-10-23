3. Under **User account recovery**, ensure that **Enable self-service account recovery – Recommended** is selected. Next, ensure that **Email only** is selected for the method of account recovery. Click **Next**.

# User account recovery

Configure how users will recover their account when they forget their password. Recipient message and data rates apply.

Self-service account recovery  Info

☑ Enable self-service account recovery - Recommended
Allow forgot-password operations in your user pool. In the hosted UI sign-in page, a "Forgot your password?" link is displayed. When this feature is not enabled, administrators reset passwords with the Cognito API.

Delivery method for user account recovery messages  Info
Select how your user pool will deliver messages when users request an account recovery code. SMS messages are charged separately by Amazon SNS. Email messages are charged separately by Amazon SES. Learn more about pricing.

+   - Email only
+   - SMS only
+   - Email if available, otherwise SMS
+   - SMS if available, otherwise email
+   - SMS if available, otherwise email, and allow a user to reset their password via SMS if they are also using it for MFA

Figure 2.19 – User account recovery

# Step 3 – Configure Sign-Up Experience

+   1. Ensure **Enable self-registration** is selected under **Self-service sign-up**.
+   2. Under **Attribute verification and user account confirmation**, ensure that **Send email message, Verify email address** is selected.
+   3. Under the **Required attributes** section, select given_name from the **Additional required attributes** drop-down menu. Click **Next**.