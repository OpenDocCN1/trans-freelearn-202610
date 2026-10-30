
## Configuring Shipping

If you go to the SALES | Shipping Methods | UPS section in the STORES | Configuration page, you will see the following form on the screen:

![](img/de3034a92496a32462134266292a51dd_312_0.png)

We will review each of the fields from this form:

- **General configuration:**
  - **Enabled for Checkout**: Specify whether UPS should be available as a shipping method on the Storefront.
  - **UPS Type**: You can choose between **United Parcel Service** or **United Parcel Service XML**. With **United Parcel Service**, you can get real-time shipping rates, and with **United Parcel Service XML**, you can get real-time rates and print labels for UPS.
- **UPS-specific configuration depending on UPS type**: The following fields depend on the **UPS Type** that you choose in the previous field that we reviewed. If you choose **United Parcel Service** as a **UPS Type**, the following fields will be available:

  - **Live Account**: Choose **Yes** to activate UPS in production. If you choose **No**, you will activate UPS as a shipping method in the test mode.
  - **Gateway URL**: The default gateway URL is included in Magento. If you need to change the gateway URL, you can do it from this field.
  - **Title:** A descriptive title for the shipping method.
  - **Packages Request Type:** You can choose **Divide to equal weight (one request)** or **Use origin weight (few requests)**.

And if you choose **United Parcel Service XML** as a UPS type, the following fields will be available:

- **Live Account:** Choose **Yes** to activate UPS in production. If you choose **No**, you will activate UPS as a shipping method in the test mode.
- **Access License Number:** Your UPS access key.
- **Gateway XML URL:** The default gateway XML URL is included in Magento. If you need to change the gateway XML URL, you can do that from this field.
- **Mode:** As you can see in the comment for the field, this setting enables or disables SSL verification of the Magento server by UPS.
- **User ID:** Your UPS User ID.
- **Origin of the Shipment:** Select one of the options from the dropdown to specify the origin of the shipment.
- **Password:** Your UPS account password.
- **Title:** A descriptive title for the shipping method.
- **Enable Negotiated Rates:** Enable negotiated shipping rates with UPS.
- **Packages Request Type:** You can choose **Divide to equal weight (one request)** or **Use origin weight (several requests)**.
- **Shipper Number:** This is a six-character UPS code, which is required for negotiated rates.
- **Tracking XML URL:** The default tracking XML URL is included in Magento. If you need to change the tracking XML URL, you can do that from this field.

**Other configuration:** The following fields are available for both **United Parcel Service** and **United Parcel Service XML UPS** types:

- **Container:** Set the container for the shipment.
- **Weight Unit:** Set the weight unit for the shipping method.
- **Destination Type:** Select whether the destination type is residential or commercial.
- **Maximum Package Weight:** Set the number for the maximum package weight in the carrier.
- **Pickup Method:** Set the pickup to any of the following methods:
  - **Regular Daily Pickup**
  - **On Call Air**
  - One Time Pickup
  - Letter Center
  - Customer Counter
- **Minimum Package Weight**: Set the number for the minimum package weight in the carrier.
- **Calculate Handling Fee**: Set whether the handling fee is a fixed amount or a percentage, based on the price defined in the previous field.
- **Handling Fee**: This will be a fixed amount or a percentage to calculate the handling fee.
- **Allowed Methods**: Select the UPS methods that will be available for selection on the Storefront.
- **Free Methods**: Select the UPS method that should be available for selection as free shipping.
- **Free Shipping Amount Threshold**: When the total order amount is above this number, the UPS method that was selected in the previous field will be available for selection as free shipping.
- **Displayed Error Message**: The error message to be displayed when the shipping method is not available.
- **Ship to Applicable Countries**: Specify whether the shipping method should be available for all countries or only specific countries. If you choose specific countries, the **Ship to Specific Countries** dropdown will be enabled to select the countries that will be available for this shipping method.
- **Show Method if Not Applicable**: Specify whether or not the shipping method should be visible on the Storefront when the method is not available.
- **Debug**: Enable debug mode to log useful information for debugging during development.
- **Sort Order**: Set the order to determine the position of **UPS** in the list of available shipping methods on the Storefront. The lower the number, the higher it will appear on the list.

Once you set up the shipping method, customers will be able to get rates from UPS to select the shipping method for their orders.

There, you will see that you can filter by Magento edition, version, extension ratings, price, and type of extension developer:

![](img/de3034a92496a32462134266292a51dd_316_0.png)

### Summary

In this chapter, we reviewed the shipping methods that are included in Magento by default. In the next chapter, we will review the process of fulfilling an order, including an overview of the life cycle of an order in Magento.

## 17 Fulfilling Orders

In this chapter, we will go through the following concepts:

- Order management
- State and status
- Invoices, shipping, and credit memos

### Order management

Magento offers a powerful yet simple way to manage the orders that are placed in your store. You can even place orders from the admin panel if a customer calls by phone and wants to buy products from your store.

You can see and manage your orders from the SALES | Orders page in the admin panel:

![](img/de3034a92496a32462134266292a51dd_318_0.png)

A very important aspect of this page is that you can select really useful mass actions to manage your orders:

![](img/de3034a92496a32462134266292a51dd_318_1.png)

As you can see, you can put several orders on hold at the same time, print invoices for several orders, and so on. Besides this, the Orders grid allows you to continue using all the utilities from the Magento 2 admin panel grids, such as the following ones:

- Filters
- Selectable columns
- Export to CSV and XML
- Search by keyword

If you click on any order from the grid, you will be redirected to the order page:

![](img/de3034a92496a32462134266292a51dd_319_0.png)

The first thing you will notice is that you have several actions for this order on the top-right corner of the screen:

![](img/de3034a92496a32462134266292a51dd_320_0.png)

You can click on these buttons to quickly perform an action for these orders—from sending the order e-mail to the customer to putting the order on hold, creating an invoice, or even editing the order.

- **Information** tab

Let's take a look at each of the sections from the **Information** tab:

| Order & Account Information |  |
|---|---|
| **Order # 000000003 (The order confirmation email was sent)** | **Account Information** |
| Order Date | Dec 26, 2016, 9:07:43 AM |
| Order Status | Pending |
| Purchased From | Main Website<br>Main Website Store<br>Default Store View |
| Placed from IP | 127.0.0.1 |
| Customer Name | Gabriel Guarino |
| Email | guarinogabriel@gmail.com |
| Customer Group | General |

Here, you can see the basic information from the order. It's essential to know the store for this order (in the case of a multistore Magento website), the customer name, e-mail, and even the IP address in case of a suspicious order.

The next two sections are **Address Information** and **Payment & Shipping Method**:

![](img/de3034a92496a32462134266292a51dd_321_0.png)

As you can see in the first section, you can edit the customer address (both shipping and billing) in case of any typo or mistake from the customer when the order was placed.
