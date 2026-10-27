# What just happened?

You have protected a part of the page content from being viewed by an unauthorized user. You have set the page as not cachable first. It is important to do it with every page that should be protected or every page that contains the login form. Cached pages would also cache the login status of the user and show him as logged in even if he is logged out.

The page should not be searchable as it would not make sense to show it in the search results to the non-authorized users.

In the Content field, the login form (if the user is not logged in) or logout form (if the user is logged in) is displayed. The form will automatically change depending on the user's status. Everything passed between the smarty tags IF is only shown to the logged in users.

In this way, you can protect every page or only a part of the content depending on whether the user is authorized or not.

# Creating user area for support requests

A support ticket system is an area where your customers can place their questions about your products or services. This area is private and cannot be seen by website visitors or other customers. When the question is placed in the support ticket area, the support staff can give the customer an answer to it. If the issue is not resolved by an answer, the customer can respond to the answer and the staff answers again. This dialog (questions and answers) is organized as one ticket. If the customer has another question, then he can open another ticket. In the private service desk, the customer can always see the list of his tickets and read the answers given from the staff. A support ticket system is often used in the professional customer care.

To create a support ticket system, you do not need a special support ticket module. Let us see how you can use the feedback module to create service desk functionality. The name of the module in question is CGFeedback. This module is developed to allow website visitors to provide comments on individual pages, news articles, products, or other items on your website. We will use the functionality of this module to create a rudimentary support ticket system.

In the admin console, click on Extensions | Module Manager, and search on the Available Modules tab for the module CGFeedback. Click on the Dependencies link to figure out what modules should be installed first. You will see the following modules:

+   - CGExtensions
+   - CGSimpleSmarty