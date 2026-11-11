For example, to search for MP3 Players on Amazon U.S., your REST request URL could look like this:

```
http://webservices.amazon.com/onca/xml
?Service=AWSECommerceService
&SubscriptionID=(Your Key Here)
&Operation=ItemSearch
&AssociateTag=(Your Tag Here)
&ResponseGroup=Large
&searchIndex=Blended
&Keywords=MP3%20Players
```

In this example, a search for items is triggered by the ItemSearch value for the Operation parameter. Everything else is required for all transactions, except the last two, searchIndex and keywords, which are required because of ItemSearch.

In its most basic form, mashups against ECS is basically a series of similarly-formatted REST requests made against ECS. The biggest challenge will be to navigate through all the available features.

# Mashing Up

We now have a little more detail about how the Internet UPC Database and ECS work beyond just knowing that one is an XML-RPC service and one is a REST service. We can start mashing up.

Our mashup will use ECS and the UPC Database services to do the following:

+   1. A form will take the UPC, short UPC, or EAN to lookup the full UPC at Internet UPC Database.
+   2. Find the price and product information using Amazon's ECS. Display this on our site.
+   3. Offer the visitor the chance to buy this item.
+   4. If they wish to buy it, add the item to a shopping cart using ECS.
+   5. Pass the user to Amazon.com for checkout.

The following code listings are included in the example code, and, provided that you substitute your own Amazon Access Key, is a functioning version of the mashup. For the sake of brevity, I will include each entire script page and comment on them as we go.