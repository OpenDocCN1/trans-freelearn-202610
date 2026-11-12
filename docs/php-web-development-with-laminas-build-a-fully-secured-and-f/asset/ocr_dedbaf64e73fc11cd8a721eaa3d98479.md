# i18n

i18n is the acronym for internationalization. In short, it is support for various languages. I18n software components help to translate content from a language to another one.

# Important note: Dependency between a form and translator

If you haven't installed the `laminas-i18n` component and you try to render a form defined by `laminas-form`, you will receive the message Class 'Laminas\\I18n\\View\\Helper\\AbstractTranslatorHelper' not found. Well, according to this message, a form requires a translator. This requirement can seem strange because it supposes that any form must be translated. Considering a hard coupling architecture, such as the Laminas components architecture, the relationship between a form component and a translator component should be optional. But the reason for the strong coupling between `laminas-form` and `laminas-i18n` is that form components can be integrated into filter and validation components and some of these components are dependent on `laminas-i18n`.

# laminas-mvc-form

If you are using `laminas-mvc` and you don't want to install `laminas-form` and `laminas-i18n` separately, you can install the `laminas-mvc-form` meta package.

At this time, we have successfully replaced the direct definition of the discount form with HTML by rendering `laminas-form`. The presentation of the discount form has not changed, so we don't have a new interface. On the other hand, we can now connect the model to the form in the controller layer.

So, we have concluded the generation of a form for adding and updating discounts using `laminas-form`. To do this, we did the following:

+   * Created a CRUD implementation for discounts, with an API and a web interface
+   * Created a class to define an HTML form for discounts

In the next section, we will learn how to filter input values using `laminas-filter`.

# Filtering input data

As you have added and installed the `laminas-form` component in this chapter using Composer, do the same for the `laminas-inputfilter` component:

+   1. Open the `composer.json` file in Eclipse.
+   2. Through the **Dependencies** tab, search for the `laminas/laminas-inputfilter` package and add it.
+   3. Don't forget to save the file and click on the **Update dependencies** button.