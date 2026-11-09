# FMA considerations

While designing your FMA strategy, it’s essential to keep a few things in mind, as follows:

+   - A fully managed package cannot be hidden or revealed using the FMA. However, you can provide a **call to action (CTA)** for users to access the features.
+   - Testing can be challenging as there’s no test LMO, and the documentation might be unclear. Developers need to perform a dark launch to the packaging environment for testing.
+   - Develop a features strategy, keeping in mind the 200-feature limit per managed package.
+   - Only three types of FMA flags are available: **Boolean**, **Integer**, and **Date**.
+   - You can adjust feature parameters in your LMO manually. However, suppose your solution has many feature parameters that must be changed from their default settings for paying customers upon installation. In that case, it might be a good idea to automate this process. Consider setting up a record-triggered flow on the License object to make things smoother.

# Unlocking the full potential of your PBO

As you already know, your PBO is a standard Salesforce org, which means it can be customized and extended to meet your specific needs through local development or by installing managed packages. To help you unlock your PBO’s full potential, let’s discuss three primary ways of extending it: installing an AppExchange app (either generic or dedicated to ISVs, such as