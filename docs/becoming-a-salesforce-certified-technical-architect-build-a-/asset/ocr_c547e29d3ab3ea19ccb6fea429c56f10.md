+   * The activities you planned for each environment and the types of tests included.
+   * Details about who will be deployed to each branch and at what stage.
+   * Make sure you fully understand the concepts of **continuous integration and continuous deployment (CI/CD)**. Hands-on experience is very important.
+   * Any third parties that you are planning to use (for example, automated build tools, source controls, and so on).

You will cover examples and more details related to this diagram in the chapters to come in this title.

It is common to combine this diagram with the optional source code branching diagram, if you decided to create one.

# The Contextual SSO Flow Diagram

Most of the CTA exam scenarios will have SSO requirements. This diagram (you might need more than one, depending on the used SSO standards) will help you walk your audience through the details of the target user experience. If you did not create this diagram as part of your presentation, then you are likely going to be asked to draw it during the Q&A session.

A standard sequence diagram is highly recommended. This is one of the diagrams I personally recommend as it is more suitable to be drawn interactively (for instance, using a whiteboard).

For this diagram, take the following into consideration:

+   * You need to know how to fully draw the following flows at the back of your hand, along with all the required details:
+       * SAML IDP initiated
+       * SAML SP initiated with deep linking
+       * OAuth 2.0/OpenID Connect web server/Auth code
+       * OAuth 2.0/OpenID Connect user-agent
+       * OAuth 2.0/OpenID Connect refresh token
+       * OAuth 2.0/OpenID Connect JWT flow
+       * OAuth 2.0 Asset token flow

You need to understand when to use each of these flows.

+   * Pay extra attention to the use cases when you need to include them as part of your integration architecture (for example, an external system authenticating into Salesforce) or mobile architecture (for example, a native app authenticating into Salesforce).
+   * Social sign-on comes with a few caveats that you need to keep an eye on and be able to explain.