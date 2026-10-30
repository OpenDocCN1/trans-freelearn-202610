
## CMS Pages, Blocks, and Widgets

There, you will see the following grid with the existing blocks:

![](img/de3034a92496a32462134266292a51dd_203_0.png)

There are 18 static blocks included in the Magento 2 sample data. Let's review the static block Training Block. Click on Select on the row of that static block in the Blocks grid and click on Edit:

![](img/de3034a92496a32462134266292a51dd_203_1.png)

You will be redirected to the following form:

![](img/de3034a92496a32462134266292a51dd_204_0.png)

As you can see, the form is simpler than New/Edit page from the CMS Pages. There are a few fields:

- **Enable Block**: Set the static block to be active in order to be included in other pages
- **Block Title**: Title for the static block
- **Identifier**: Unique identifier for the static block
- **Store View**: The page will be visible in the selected store views
- **WYSIWYG Editor**: You can edit the content of the static block using the WYSIWYG editor

This is the banner included in the Training category page. You can see the banner if you click on the Training category in the main navigation menu:

![](img/de3034a92496a32462134266292a51dd_205_0.png)

The Static Blocks can be included in the following places:

- Template files in the theme (included programmatically)
- Categories
- CMS Pages and other static blocks through widgets

## Widgets

Widgets are modules that can be included in different pages to display the information based on different configurations through different widget types.

The following widgets are available in Magento for content management:

- CMS Page Link
- CMS Static Block
- Catalog Category Link
- Catalog New Products List
- Catalog Product Link
- Catalog Products List
- Orders and Returns
- Recently Compared Products
- Recently Viewed Products

You can manage your widgets by going to the CONTENT | Element | Widgets section in the admin panel:

![](img/de3034a92496a32462134266292a51dd_206_0.png)

There are 18 widgets included in the Magento 2 sample data. As you can see in the following screenshot, all of them are CMS Static Blocks that are included in different places on the Storefront:

## CMS Pages, Blocks, and Widgets

![](img/de3034a92496a32462134266292a51dd_207_0.png)

We will create a new widget by clicking on the Add Widget button on the top-right corner of the screen. The following form will be displayed:

![](img/de3034a92496a32462134266292a51dd_207_1.png)

You can see the conditions for the widget in the following screenshot:

![](img/de3034a92496a32462134266292a51dd_210_0.png)

Finally, click on **Save** to enable the widget. You will see Magento notifying that you need to refresh the cache:

![](img/de3034a92496a32462134266292a51dd_210_1.png)

You should clear the cache types from the Cache Management page as we saw before. After that, you will see the widget in the anchor categories, for example, Men | Tops:

![](img/de3034a92496a32462134266292a51dd_211_0.png)

## Summary

In this chapter, you learned how to manage the content in your Magento store, including CMS Pages, static blocks, and widgets. In the next chapter, we will review how to manage the scope of your websites, stores, and store views, and we will review how to configure the locale options in your Magento store.

## 9
### Managing Scope and Locale Settings

In this chapter, we will cover:

- Magento scopes – websites, stores, and store views
- Reviewing how adding store views affects the catalog and CMS
- Languages

## Magento scopes – websites, stores, and store views

One of the most important features of Magento is the ability to create multiple stores from a single Magento instance. Magento has implemented a hierarchical system that includes four levels:

- Default/global
- Website
- Store/store group
- Store view

The most common scenario of multistore configuration in Magento is found in retailers that separate their catalog by store view as follows:

![](img/de3034a92496a32462134266292a51dd_213_0.png)

In the preceding example, the company has a website, but each product family has its own store to target different customers. Finally, each store has its own store views to be able to display the content in different languages.

With a single instance of Magento, you can implement a multistore configuration using:

- Multiple languages
- Multiple domain names
- Different categories

## Managing Scope and Locale Settings

You can see the store configuration for your Magento instance in the STORES | Settings | All Stores section of the Magento admin panel. You will see the following grid on the screen:

![](img/de3034a92496a32462134266292a51dd_214_0.png)

There you will see the current store configuration of your Magento instance. Magento includes one website, one store, and one store view in the Magento 2 sample data. In the subsequent sections, we will review all the concepts, from websites to store views.

## Websites

A website contains one or more stores. You can have a different set of customers in different websites, all of them managed from a single Magento instance.
If not, you can share the customer accounts between all the websites. In addition to that, you can set specific product prices, tax classes, currencies, and system configurations by website.

## Stores/store groups

A store contains products and categories. Each store has an associated root category, allowing you to define different categories by store. In the previous example, we saw two stores with completely different product families:

![](img/de3034a92496a32462134266292a51dd_215_0.png)

Magento allows you to do this by defining a root category per store. All the stores that are associated with the same website share the same customer accounts.

## Store views

Store views are commonly used to define different languages for your website. By doing so, the merchants are able to target customers by their language.

## Managing Scope and Locale Settings

All the store views that are associated with the same store share the same root category, shipping, and payment methods. By using store views, you can set different prices, titles, descriptions, and more.

We will review the **Web Site**, **Store**, and **Store View** that is included in the Magento 2 sample data.

We will go to the **STORES** section in the admin panel: **STORES | Settings | All Stores.**

There, you will see the grid that we described before:

![](img/de3034a92496a32462134266292a51dd_216_0.png)

## Managing Scope and Locale Settings

We will start by clicking on the Main Website link on the first column. You will be redirected to the following screen:

![](img/de3034a92496a32462134266292a51dd_217_0.png)

There you can edit the Name, Code, Sort Order, and Default Store. Take into account that the multistore configuration can be set in the index.php file in the root of the project. If the multistore configuration has been set in that way and you change the website code, make sure to update the index.php file to specify the new code for the website.

## Managing Scope and Locale Settings

If we go back to the **STORES** grid and click on the **Main Website Store** link in the second column, we will be redirected to the following page:

![](img/de3034a92496a32462134266292a51dd_218_0.png)

As you can see on that page, you can see the parent website for this store, edit the **Name**, and set the **Root Category** and the **Default Store View**.

Finally, if we go back to the grid and click on the Default Store View link in the third column, we will be redirected to the following form:

![](img/de3034a92496a32462134266292a51dd_219_0.png)

You can edit the parent store for the store view, set the Name, Code, Status, and Sort Order. It's important to mention that the multistore configuration can be set using the store view code in the index.php file. If that is the case, you should update the file every time you change the store view code in the admin panel.

## Default/global scope

Besides the Web Site, Store, and Store View, we should mention that the default/global scope defines the default values for different configurations in the admin panel. By selecting this scope, you will set the specific configuration to be applied to all websites, stores, and store views.

When you are editing the Magento configuration, products, or categories, you will be able to select the scope for the values. You can define the values globally, or, specifically, on a website, store, or store view level.
