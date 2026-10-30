
## Summary

In this chapter, we went through the most important system configuration fields in Magento. In the next chapter, we will go through the process of managing customer accounts and customer groups to be able to create, edit, and delete accounts, and associate them with different groups.

## 11
## Working with Customers

In this chapter, we will cover the following topics:
- Working with customer accounts
- Customer groups

### Working with customer accounts

Customers have the ability to register an account on the Storefront to keep track of their orders, save their personal information for future orders, and more.

You can manage the customer accounts in your store from the CUSTOMERS | All Customers section in the admin panel.

## Working with Customers

You will see the following page on the screen:

![](img/de3034a92496a32462134266292a51dd_244_0.png)

One of the most useful elements from the grids in the Magento admin panel is the Actions dropdown. The dropdown contains mass actions to apply a specific action to the items that are selected in the grid.

The following mass actions are available from the Customers grid:
- Delete
- Subscribe to Newsletter
- Unsubscribe from Newsletter
- Assign a Customer Group
- Edit

In addition to that, you can filter and sort the list of customers in the grid, set the number of customers to display per page, select the columns to display, search by keyword, and export the customer list to CSV and Excel XML.

## Working with Customers

You can view and edit the customer information by clicking on the **Edit** link in the last column from the **Action** grid.

If you click on that link, you will be redirected to the **Customer** edit page:

![](img/de3034a92496a32462134266292a51dd_245_0.png)

We can see that there are several buttons on top to perform different actions on the customer account:
- **Delete Customer**
- **Reset**: Resets all changes that have been made in the customer information since you opened the **Customer** edit page
- **Create Order**: Creates an order for the customer
- **Reset Password**
- **Force Sign-In**: Revokes the API token for the customer account

On the left sidebar, the following sections are available:
- **Customer View**: Take a look at the following screenshot:

![](img/de3034a92496a32462134266292a51dd_246_0.png)

You can see the personal information of the customer, including:
- The last time the customer logged in
- Whether the account is locked or unlocked (Magento locks the account after six failed login events)
- If a confirmation e-mail was required, then you can see whether the account has been verified
- The date that the account was created on
- The store view from which the account was created
- The customer group of the customer account
- The default billing and shipping addresses

- **Account Information**: You can edit the personal information in this section and specify the customer group for the customer account:

![](img/de3034a92496a32462134266292a51dd_246_1.png)

- **Addresses:** You can add new addresses for the customer account and specify which one of them should be the default billing and shipping addresses:
- **Orders:** You can see the list of orders placed by the customer. You can open the order to see all the details and even reorder directly from the grid (if reorder is enabled in the system configuration–SALES | Sales | Reorder | Allow Reorder):

![](img/de3034a92496a32462134266292a51dd_247_0.png)

![](img/de3034a92496a32462134266292a51dd_247_1.png)

- **Billing Agreements**: To make the checkout process simpler, customers can enter into a billing agreement with PayPal as the payment service provider. The customer chooses the billing agreement as the payment method in the checkout, and PayPal identifies the billing agreement by its unique number and proceeds with the charge. The **Billing Agreements** section contains all the billing agreements for the customer:

![](img/de3034a92496a32462134266292a51dd_248_0.png)

- **Newsletter**: You can check if the customer is subscribed to the newsletter and see the list of newsletters with the ability to filter by **ID**, **Start date**, **End Date**, **Receive Date**, **Subject**, and **Status**:

![](img/de3034a92496a32462134266292a51dd_248_1.png)

- **Product Reviews:** You can see the list of product reviews written by the customer:

![](img/de3034a92496a32462134266292a51dd_249_0.png)

- **Wish List:** You can review the list of products that are in the wish list of the customer, including important information such as the number of days since the product was added to the wish list:

![](img/de3034a92496a32462134266292a51dd_249_1.png)

## Adding a new customer

Even though the customer can create the account from the Storefront, the administrator can create the account from the admin panel as well. You can add a new customer by clicking on the Add New Customer button in the Customers page (Customers | Customers | All Customers):

![](img/de3034a92496a32462134266292a51dd_250_0.png)

Now, click on the **Save Customer** button on the top-right corner of the screen. You will be redirected to the **Customers** page, and you will see the success message as confirmation that the account has been created:

![](img/de3034a92496a32462134266292a51dd_252_0.png)

## Summary

You saw that managing customers in Magento is really simple, from the ability to see the list of all customers to applying useful mass actions such as *Subscribe to Newsletter*. Creating and editing customers is an easy process as well, and you can review all the information of customers, including their orders, reviews, and more.

In the next chapter, we will go through the process of setting up and managing taxes for your Magento store.

## 12
## Admin Users and Roles

In this chapter, we will cover the following concepts:
- Managing admin users
- Managing roles and granting permissions

### Managing admin users

When managing an e-commerce website, you need different administrators that have access to different pages in the admin panel. For example, if a person from your team is in charge of adding and updating products in your Magento store, then you need that person to access only the **PRODUCTS | Catalog** page. If another team member is in charge of marketing, then you can create a new admin user with a specific role to access just the **MARKETING** and **CONTENT** sections.

## Admin Users and Roles

You can create users for the Magento admin panel and grant specific privileges to each of the user roles. You can see a list of existing admin users by opening the SYSTEM | Permissions | All Users page in the admin panel:

![](img/de3034a92496a32462134266292a51dd_254_0.png)

You can add new users by clicking on the Add New User button on the top-right corner of the screen. You will be redirected to the following form:

![](img/de3034a92496a32462134266292a51dd_254_1.png)

## Admin Users and Roles

There, you can set the personal information for the new admin user, the password, the interface language, the account status, and the admin role.

If we take a look at the User Role tab, we can see that Magento includes only one user role by default, that is, Administrators:

![](img/de3034a92496a32462134266292a51dd_255_0.png)

Let's create a sample admin user by adding the following information to the form:

### User Info:
- **First Name**: Your first name
- **Last Name**: Your last name
- **Email**: Your e-mail address
- **Password**: Set a password for the account
- **Password Confirmation**: Repeat the same password for the account
- **Interface Locale**: English (United States) / English (United States)
- **Your Password**: Password for the current admin user
