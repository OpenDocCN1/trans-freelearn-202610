# # Implementing a Product Basket

This chapter covers the implementation of a product basket, an essential component of a virtual store. In _Chapter 9, Event-Driven Authentication_, we learned about the `laminas-session` component and how it can be used to manage sessions and persist data between HTTP requests. In this chapter, we will use `laminas-session` to implement a product basket.

We will first start by learning how to implement inventory management. Next, we will understand how to refactor the customer home page with view helpers. After this, we will implement a product basket. Lastly, we will learn how to make a purchase order for an authenticated customer.

At the end of this chapter, you will be able to select products from a listing page, add these products to a basket, remove products from this basket, and make a purchase order. This purchase process includes customer registration, which is very similar to the employee registration we implemented in _Chapter 9, Event-Driven Authentication_.

In this chapter, we'll be covering the following topics:

+   - Managing the product inventory
+   - Refactoring the customer home page
+   - Controlling the product basket
+   - Controlling the customers
+   - Decoupling the generic behavior of authentication

# ## Technical requirements

All the code related to this chapter can be found at `https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter11/whatstore`.